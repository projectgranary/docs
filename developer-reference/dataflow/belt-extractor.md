---
description: A function-as-a-service like Python callback runtime
---

# Python Belt Framework

Since Granary 1.2 there is a second Belt type called dbt-segment available. This page only deals with the Python Belt.

![Data flow within harmonized data zone of Granary](../../.gitbook/assets/dataflow\_profile.PNG)

Belts are most commonly used to compute updates for profile entities stored in the [Profile Store](profile-store/). These updates can represent things like:

* whether a certain user has reached a goal in a conversion funnel
* what device type or operating system was used for the most recent visit
* when the most recent visit happened
* the increment on a counter that represents the number of visits per week
* a link between an anonymous session profile and an authenticated user
* the addition of a contract the user has and what the value of that contract is
* etc.

They are either defined by a stateless Python function that gets invoked for every event received from the respective [Event Type](../../learning-grnry-1/data-in/how-to-run-a-harvester/event-types.md) input.The function typically extracts data from the payload in order to compile one or more update statements for the Profile Store. This page describes the Python framework.

## Input Data Format

The Belt framework can consume messages from one or many Event Types. Technically, messages in the Event Type's Kafka topics need to have the following event headers and an arbitrary event payload:

{% tabs %}
{% tab title="Spec" %}
| Key                            | Description                                                                    |
| ------------------------------ | ------------------------------------------------------------------------------ |
| **event\_headers**             | Kafka fields for Event metadata                                                |
| $$-$$ grnry-event-type         | event type as specified during harvester definition                            |
| $$-$$ grnry-event-id           | used to deduplicate events                                                     |
| $$-$$ grnry-harvester-name     | name of the harvester instance, extracted from event payload or a static value |
| $$-$$ grnry-correlation-id     | used to group events received from the same tracking entity                    |
| $$-$$ grnry-event-timestamp    | event processing time set by harvester (metadata extractor)                    |
| $$-$$ grnry-event-type-version | version of event type registered with the harvester                            |
| **event\_payload**             | Forwarded from input attribute `value`                                         |
| $$-$$ schema                   | Snowplow Event Schema Reference                                                |
| $$-$$ ipAddress                | ipAddress if Snowplow is configured to collect this                            |
| $$-$$ timestamp                | time of event creation or reception(?)                                         |
| $$-$$ collector                | identifies the source platform of the event                                    |
| $$-$$ body                     | Snowplow Event Data (according to `schema`)                                    |
| $$-$$ headers                  | HTTP headers                                                                   |
{% endtab %}
{% endtabs %}

## Output Data Format

### Profile Update

Profile update messages need to comply to the following output data format which is explained in detail in the following paragraphs.&#x20;

{% tabs %}
{% tab title="Spec" %}
see [Profile specification](https://github.com/syncier/grnry-kafka-profile-update/blob/master/PROFILESPECS.md)

| Key             | Description                                                                                                                                                                                                                                                              |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `_operation`    | update operation, default is `_set`, see [Profile Store](profile-store/#component-profile-updater) for more information                                                                                                                                                  |
| `_id`           | identifies the profile that should be updated with this message                                                                                                                                                                                                          |
| `_path`         | The path within the nested structure of a profile that should be updated. In case the path doesn't exist yet it will be created. An array of length >= 1                                                                                                                 |
| `_value`        | _Mandatory_. The grain value that should be set in the profile under the defined `_path` _._ Needs to be set by `set_value()`. For `_delete` operation `_v`is used to specify an array of pits to be deleted from that path. `""`, `[""]` or `[]` will delete `_latest`. |
| `_profile_type` | Categorizes the profile to be updated. Default is `_d`.                                                                                                                                                                                                                  |
{% endtab %}
{% endtabs %}

See exented example in below's [Python callback function](belt-extractor.md#python-callback-function) section. Apart from the profile update, there are three other update classes available.&#x20;



### Granary Event Update

The `GrnryEventUpdate` class can be used to send messages to a [data\_in ](event-type.md#data-in)event type. This can be useful while chaining multiple belts. The constructor's signature contains the parameters as defined in the [Input Data Format ](belt-extractor.md#input-data-format)above. After that, set the message payload using the 'set\_value' method and add the update to the `updates` array that will be returned by the belt at the end of the event processing.

{% tabs %}
{% tab title="Spec" %}
| Key              | Description                                                                                                                                                     |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| output type name | The field is the event type name. There needs to be a matching output `data_in` event type defined in the Belt model. If not, messages go to dead letter queue. |
| id               | The field is the correlation id                                                                                                                                 |
| event id         | This field contains the event id. Defaults to event id of the incoming message. (optional)                                                                      |
| timestamp        | This field contains the event's timestamp. Defaults to the timestamp of the incoming message. (optional)                                                        |
| headers          | This field contains the Kafka message headers of the event in the json format. Defaults to the headers of the incoming message. (optional)                      |


{% endtab %}

{% tab title="Example" %}
```python
from grnry_belt.models.grnry_event_update import GrnryEventUpdate

def execute(event_headers, event, profile=None):
    correlation_id = event_headers['grnry-correlation-id']
    updates = []
    msgPayload = {
        'contract' : 'foobar'
    }

    dataInUpdate = GrnryEventUpdate('dataInOutput', correlation_id)
    dataInUpdate.set_value(msgPayload)
    updates.append(dataInUpdate)
    return updates
```


{% endtab %}
{% endtabs %}

&#x20;

### DirectUpdate

The `DirectUpdate` class can be used to send messages to a [live\_segment ](event-type.md#live-segment)event type.

{% tabs %}
{% tab title="Spec" %}
| Key              | Description                                                                                                                                                          |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| output type name | The field is the event type name. There needs to be a matching output `live_segment` event type defined in the Belt model. If not, messages go to dead letter queue. |
| value            | This field is the value as a Python dict matching the JSON Schema of the mentioned `live_segment` event type. Needs to contain an id property.                       |


{% endtab %}

{% tab title="Example" %}
```python
from grnry_belt.models.direct_update import DirectUpdate

def execute(event_headers, event, profile=None):
    updates = []

    value={
        'id' : '2',
        'test': 'Output_sample'
    }
    liveSegmentUpdate = DirectUpdate('liveSegmentOutput', value)
    updates.append(liveSegmentUpdate)

    return updates

```
{% endtab %}
{% endtabs %}



### **Generic Update**

{% hint style="warning" %}
From Granary 1.2 onwards, this update is deprecated. It does not work with the `outputTypes` parameter, only when using the `kafkaDestinationTopic` parameter.
{% endhint %}

Generic update message write custom messages to the Belt's Kafka output topic.

{% tabs %}
{% tab title="Spec" %}
| Key     | Description                                                              |
| ------- | ------------------------------------------------------------------------ |
| id      | The field id is the correlation id                                       |
| value   | This field content the payload of the event in the json format           |
| headers | This field content the header of the event in the json format (optional) |
{% endtab %}

{% tab title="Example" %}
```python
import datetime

from grnry_belt.models import generic_update

eventHeader = { "grnry-harvester-name": "my-harvester", "grnry-event-type": "my-event-type", "grnry-correlation-id": "abc","grnry-event-timestamp": str(int(datetime.datetime.now().timestamp())),"grnry-event-id": "xyz", "grnry-event-type-version": "1"}
eventPayload = { "data": "my-data" }
generic_update.GenericUpdate(correlationId, eventPayload, event_headers)

```
{% endtab %}
{% endtabs %}

****

## **Python callback function**

The user supplied Belt Python callback function needs to convert the input messages from above to one or many so called **profile updates** (it is also valid to not return any update). These updates materialize as **grains** in the Profile Store. Many grains shape a **fragment** whereas many fragments from a **profile,** hence **Profile Store**.

The Python code provided here has to be in the form of a function by the name **callback.py** that can be invoked using the signature `execute(event_headers, event_payload)` where the parameter `event_payload` and `event_headers` are both a Python dictionary containing one event/header from the input topic or a list of dictionaries containing multiple events/headers. See [Belt Callback Signatures](../../learning-grnry-1/using-data-in-granary/best-practices/belt-callback-signatures.md) for more complete examples.

A simple callback example:

```python
from time import time

def execute(event_headers, event_payload, profile=None):
    print(profile)
    # Create Profile Update object with correlationId (String) and path (Array<String>)
    update = Update(profile['correlationId'],["dummy"])
    # Set value of Profile Update
    update.set_value("Hello Belt!",0.5,time(),'P1D','Dummy-Belt')
    # Set type of Profile Update
    update.set_type('TestProfileType')
    # Set operation of Profile Update
    update.set_operation('_set')
    return [update]
```

{% hint style="info" %}
The **Update** object is injected into the callback and does **not** require an additional import statement.
{% endhint %}

The function can return either `None`, `[]` or a list of `Update` objects representing an update to a profile in the profile store. If the return is not an Update object the belt will continue with the next message. In case it is, the object needs to be following below schema:

```yaml
UPDATE :=
  {
    "_schema": string,                          # schema of upddate message, default is null
    "_operation": a valid update operation,     # default is "_set"
    "_id": string,
    "_path": \[ string [,string]* \],           # array of length >= 1
    "_value": GRAIN_VALUE,                      # must not be null
    "_profile_type": string                     # default is "_d"
  }
```

Valid update operations are specified in full detail in the [Profile updater's documentation](profile-store/#component-profile-updater) and a valid `GRAIN_VALUE` is defined as follows:

```yaml
GRAIN_VALUE :=
  {
    "_v": string,                               # Json object string, Json array string, or string of counter value. Value type must fit _operation
    "_c": double,                               # default is 1
    "_in": long,                                # default is time()
    "_ttl": string,                             # period of time, https://en.wikipedia.org/wiki/ISO_8601#Durations, default is "P100Y"
    "_ttn": string,                             # period of time, https://en.wikipedia.org/wiki/ISO_8601#Durations, default is "P100Y"
    "_origin": string,                          # default is "/belts/{belt-id}"
    "_reader": string                           # default is "_auth"
  }
```

Based on the `Update` object generated by the callback, one ore more JSON update(s) will be written to the profile update topic. Every update object returned by this function must have **at least** the following attribute set:

| Key      | Method                                                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| -------- | -------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `_id`    | `Update(id, [path])`                                           | Specifies the profile that should be updated. (part of constructor)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `_path`  | `Update(id, [path])`                                           | The path within the nested structure of a profile that should be updated. In case the path doesn't exist yet it will be created. (part of constructor)                                                                                                                                                                                                                                                                                                                                                                                                           |
| `_value` | `set_value(value, certainty, _in, _ttn, origin, reader, _ttl)` | <p>Contains the value that should be set in the profile under the defined <code>_path</code>. <code>set_value()</code> expects the actual value <code>_v</code> and optional <code>_c</code>, <code>_in</code>, <code>_ttl</code>, <code>_ttn,</code> <code>_origin</code>, <code>_reader</code>. </p><p><strong>Example</strong> of a <strong>default grain</strong> value with only <code>_v</code> set:<br><code>{ "_v": "foo-bar", "_c": 1.0, "_in": time(), "_ttl": "P100Y", "_ttn": "P100Y", "_origin": "/belts/{belt-id}", "_reader": "_auth"}</code></p> |

If unspecified in the `Update` object, default values will be used:

| Attribute Name  | Method                     | Default  |
| --------------- | -------------------------- | -------- |
| `_schema`       | `set_schema(schema)`       | `null`   |
| `_operation`    | `set_operation(operation)` | `"_set"` |
| `_profile_type` | `set_type(profile_type)`   | `"_d"`   |

### **Callback Validation**

The provided callback will be validated on startup of the Belt. In case of errors the Belt will provide a log and transition into FAILED state.

Following rules are validated:

* A method called `execute` needs to be present in callback
* The execute method can only have 2 or 3 parameters
* If `FETCH_PROFILE` is `lazy` or `false`&#x20;
  * The execute method can have only 2 parameters
  * In case of a 3rd parameter a default needs to be provided e.g. `profile=None`&#x20;
* If `FETCH_PROFILE` is `prefetch` or `prefetch_with_history`
  * The execute method needs to have exactly 3 parameters

### Data **Fetching**

#### **Profile Store**

In some cases it is necessary to fetch a profile from the Profile Store using the [Profile API](../api-reference/profile-store-api.md). This can be done by fetching the profile for every event based on the `correlation_id` (`FETCH_PROFILE=prefetched or FETCH_PROFILE=prefetch_with_history`) or by injecting the `profileClient` class into the callback module (`FETCH_PROFILE=lazy`). In case of a "lazy fetch" the belt is able to use the `getProfile()` method with the following signature:

```python
def getProfile(self, correlation_id, profile_type=None, fragments=None, withHistory=None)
```

Required parameters are `correlation_id` of type `str` and `withHistory` of type `bool`.

```python
profile = profileClient.getProfile(correlation_id=event['metadata']['grnry-correlation-id'], withHistory=False)
```

The profile client uses the `PROFILE_TYPE` environment variable as default value for `profile_type`. To fetch a profile of a different profile type, the type can be passed as an optional parameter like this `getProfile(my_cid, my_profileType, withHistory=False)`.

Additionally, it is possible to fetch specific fragments of a profile by setting another optional parameter of `getProfile` like this:

```python
profile = profileClient.getProfile(event['metadata']['grnry-correlation-id'], fragments=['/customer/name','/customer/adress','/invoiceDetails'], withHistory=False)
```

Similar to profile retrieval, it is possible to fetch single grains using the `getGrain()` method that is defined as follows:

```python
def getGrain(self, correlation_id, profile_type=None, path='', pagesize=50, offset=0, withHistory=False)
```

Required parameters are `correlation_id` and `path` of type `str`.

```python
profile = profileClient.getGrain(event['metadata']['grnry-correlation-id'], '/full/path/to/grain')
```

The profile client uses the `PROFILE_TYPE` environment variable as default. To fetch grains of another profile type, the type can be passed as a further optional parameter like this `getGrain(correlation_in=my_cid, profile_type=my_profileType).`

Additionally, query parameters can be used in the method: `pageSize`, `offset` and `withHistory`. The latter allows the retrieval of grain for latest point in time or of all grain versions paginated.

```python
profile = profileClient.getGrain(event['metadata']['grnry-correlation-id'],pageSize=50,offset=0,withHistory=False)
```

Also, in order to get results, the keycloak user that is available in the belt environment must have the required keycloak roles as defined in the profilestore field `'reader'`. Otherwise, it won't get back any results. If you have an existing keycloak user with their credentials stored in a kubernetes secret, you can make the credentials available to the belt by adding the secret to your belt definition via [belt api](../api-reference/belt-api.md#create-and-store-a-belt-python-callback).

#### Event Store

Next to the `profileClient`, an `eventstoreClient` class is injected into the callback if using`FETCH_PROFILE=true` or `FETCH_PROFILE=lazy`. The eventstore client has two methods:

1. `getEvents` to fetch all events for a `correlation_id`&#x20;
2. `getEvent` to fetch a single Event with `correlation_id` and `event_id`.

```yaml
eventstoreClient.getEvents(event['metadata']['grnry-correlation-id'],offset=0,pagesize=20)
eventstoreClient.getEvent(event['metadata']['grnry-correlation-id'],'0cc10a49-2a79-4fe9-92c8-56ea92e5c281')
```

If not using the [Belt API](../api-reference/belt-api.md) as deployment path, you can configure the profile fetching using the following environment variables:

| Environment Variable         | Description                                                                                                                                        | Default |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `FETCH_PROFILE`              | Enable profile fetching. Available values: `false`, `prefetch`, `prefetch_with_history`                                                            | `false` |
| `PROFILE_URL`                | Profilestore base url                                                                                                                              | -       |
| `EVENTSTORE_URL`             | Eventstore base url                                                                                                                                | -       |
| `PROFILE_TYPE`               | Default profile type to fetch                                                                                                                      | `_d`    |
| `PROFILE_PASS_ON_EXCEPTIONS` | Every API call that has a non `200` return code will throw an exception that needs to be fetched in the belt code, otherwise it is send to the DLQ | `false` |
| `PROFILE_TIMEOUT`            | Time to wait for an answer of Profile / Eventstore API in seconds                                                                                  | `3`     |
| `KEYCLOAK_URL`               | Keycloak url (with '/auth')                                                                                                                        | -       |
| `KEYCLOAK_REALM`             | Realm of GRNRY installation                                                                                                                        | -       |
| `KEYCLOAK_CLIENT`            | Client of profile api                                                                                                                              | -       |
| `KEYCLOAK_SECRET`            | Kubernetes Secret name with Keycloak user credentials                                                                                              | -       |
| `KEYCLOAK_USER`              | Attribute name in Kubernetes Secret for Keycloak user name                                                                                         | -       |
| `KEYCLOAK_PASS`              | Attribute name in Kubernetes Secret for Keycloak user's password                                                                                   | -       |

### **Callback Timeout**

If a function is taking too much time to process a record, it will timeout to prevent errors like infinite loops. The Belt retries and does not recover if the processing time keeps exceeding the timeout. The maximum duration for the function's execution can be configured. Another time limit monitors how often long-running callbacks occur. The latter limit should be smaller than the function timeout. Both timeouts can be configured using environment variables in Belt via the [Belt API](../api-reference/belt-api.md)'s parameter `extraEnvs`:

| Environment Variable       | Default |
| -------------------------- | ------- |
| `CALLBACK_TIMEOUT_SEC`     | `300`   |
| `CALLBACK_LONGRUNNING_SEC` | `180`   |

### Retry

If the callback raises a `RetryException`, the processed message will be retried with exponential backoff until the maximum number of retries is reached. The retry behavior can be configured by setting environment variables within the belt container via [Belt API](../api-reference/belt-api.md)'s POST parameter `extraEnvs`:

| Environment Variable                  | Description                                                                                         | Default |
| ------------------------------------- | --------------------------------------------------------------------------------------------------- | ------- |
| `RETRY_BACK_OFF_INITIAL_INTERVAL_SEC` | initial retry wait time in seconds                                                                  | `1`     |
| `RETRY_BACK_OFF_INTERVAL_MAX_SEC`     | maximum retry wait time in seconds, this parameter must be aligned with Kafka consumer timeouts     | `60`    |
| `RETRY_BACK_OFF_MULTIPLIER`           | base of multiplier for exponential backoff                                                          | `2.0`   |
| `RETRY_MAX_RETRIES`                   | maximum number of retries. Disable by setting it to `0`. A value of `-1` enables unlimited retrying | `-1`    |

{% code title="callback.py" %}
```python
from grnry_belt.exceptions.exception import RetryException

def execute(event_headers, event, profile=None):
    raise RetryException('Try again')
```
{% endcode %}

## Dead letter queue

In GRNRY, we have created so called _dead letter queues_. The Belt's dead letter queue is used to receive all the data that could not be processed correctly.

By default the Belt's dead letter queue is set by the belt api to `grnry_belt_dlq_<belt-id>`. E.g.:

```yaml
 grnry_belt_dlq_42
```

In order to rename it one must change the helm deployment parameter:

```yaml
extraEnv:
  - name: KAFKA_ERROR_TOPIC
    value: <my-dead-letter-queue-name>
```

#### When is something written to the Dead Letter Queue?

The Belt writes events to dead letter queue in case of exceptions are thrown during event processing within the belt framework, e.g. if an error occurs during the en-/decryption of messages. Exceptions thrown within the callback function are written into dead letter queue, logged and the exception counter is increased. Moreover, if other exceptions other than RetryException are raised, the `RETRY_MAX_RETRIES` is set to `0`, i.e. the message is will not be retried any longer.

## Profile Linking

In Grnry Profile Linking is a feature to allow the resolving of incoming Profiles from the old correlation id to a newer one. This is useful for cases where the correlation id is derived from changing customer data, e.g. the hash of a contract number, which is then changed over time.

The linking means for incoming Profiles, the Profile Store will be checked to see if a \`Profile Link\` grain exists, if so the value (new correlation id) will be retrieved, and instead the operation will be performed instead on the new correlation id.

Furthermore `Kafka Partition Redirection` is implemented inside the Belt Framework. This ensures that when a Profile Link exists, the belt operations set the header of the Kafka message to the new correlationId, this ensure the operations on the Old/New correlation id will be sent to the same Kafka partition. Therefore processed by the same Kafka Profile Updater instance, ensuring greater consistency.

Note: For safety `Profile Operations` & `Kafka Partition Redirection` will not be performed on the `delete` or `profile_update` operations.

In order to create the Profile Link, the `_profile_link` operation should be used from one of the belts, this then creates a Profile Link grain with the following values:

| Profile Store Column Name | Value              |
| ------------------------- | ------------------ |
| profile\_type             | profile\_link      |
| path                      | /new               |
| pit                       | \_latest           |
| correlation\_id           | Old correlation id |
| value                     | New correlation id |
