---
description: >-
  This page will give you some background information about event types and how
  to make them available to harvesters.
---

# Event Types

## Event Type Definition

Event Types categorize incoming data objects \(events\) by certain properties that are crucial for further processing. An Event Type entity has a unique name and provides rules on how to extract important metadata \(`correlationId`/`eventId`\) from events of that type.

## Creating an Event Type \(also see[ API Docs](../../../developer-reference/api-reference/harvester-api.md#create-an-event-type)\)

Creating an event type is the initial step to import data into Granary.

{% hint style="info" %}
Before creating create a harvester, you need to specify the event type of the data that the harvester will import and also register a [source type](../../../operator-reference/installation/with-helm/harvester-api/source-types.md) specifying a source app implementation.
{% endhint %}

An event type entity defines rules to extract metadata from incoming data objects. All objects \(json documents\) of the same event type share a common structure. A harvester will extract metadata from these json documents by using [json path evaluation](../best-practices-1/best-practices.md#simple-json-path-evaluations) of Spring Expression Language \([SpringEL](../best-practices-1/best-practices.md#spring-expression-language-spel)\). In case some metadata fields cannot be extracted from the data itself, you can set a [registered functions](../best-practices-1/best-practices.md#use-registered-functions) as the respective expression that generates a suiting value. For each Granary-header an expression needs to be provided.

| Attribute | Description |
| :--- | :--- |
| name | The name of an event type can only consist of the characters `a-z` , `A-Z` , `0-9` and `-` . Its length can not exceed x chars due to technical limitations. |
| correlationIdExpression | A SpringEL rule to extract the correlation id from the event \(or create it using a function\). Granary-header: `grnry-correlation-id`. |
| eventIdExpression | A SpringEL rule to extract the event id from the event \(or create it using a function\). Granary-header: `grnry-event-id`. |
| timestampExpression | A SpringEL rule to extract the timestamp \(milliseconds since 1.1.1970 UTC\). _Optional_, if not set the current time is used. Granary-header: `grnry-timestamp`. |
| description | Description of this specific event type. _Optional_, default is `""`. |

#### Example Call - create event type

```bash
curl -X POST -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @event-type.json \
 https://grnry-host/event-types
```

{% code title="event-type.json" %}
```text
{
  "name" : "snowplow-web",
  "correlationIdExpression" : "#jsonPath(#jsonPath(payload, 'body'), 'data[0].correlationId')",
  "eventIdExpression" : "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].eventId')?:#randomUUID()",
  "timestampExpression" : "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].timestamp')?:#nowMillis()"
}
```
{% endcode %}

## Read  Event Type configuration \(also see [API Docs](../../../developer-reference/api-reference/harvester-api.md#update-an-event-type)\)

{% hint style="info" %}
If you did not specify all parameters on your initial POST \(event type creation\) you will still get a complete definition with all fields set \(with default values\) on event type retrieval.
{% endhint %}

#### Example Call - read event type configuration

```bash
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @event-type.json \
 https://grnry-host/event-types/snowplow-web
```

{% code title="event-type.json" %}
```text
{
    "_links": {
        "self": {
            "href": "https://development.analytics.ventures.syncier.cloud/event-types/foo-test"
        }
    },
    "eventTypes": [
        {
            "name": "snowplow-web",
            "displayName": "snowplow-web",
            "version": 1,
            "topicName": "grnry_data_in_snowplow-web",
            "correlationIdExpression" : "#jsonPath(#jsonPath(payload, 'body'), 'data[0].correlationId')",
            "eventIdExpression" : "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].eventId')?:#randomUUID()",
            "timestampExpression" : "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].timestamp')?:#nowMillis()"
            "retentionMs": 3456000000,
            "partitionCount": 32,
            "replication": 2,
            "eventstoreTTL": "P100Y",
            "description": "",
            "_links": {
                "self": {
                    "href": "https://grnry-host/event-types/snowplow-web/1"
                }
            }
        }
    ]
}
```
{% endcode %}

## Updating an Event Type \(also see [API Docs](../../../developer-reference/api-reference/harvester-api.md#update-an-event-type)\)

Existing event types can be updated. Changes of the incoming data's structure may require adjustment of SpringEL expressions to determine the `correlationId`/`eventId` etc. Each update will be stored as a new version \(auto incremented number starting at 1\). 

The definition of a harvester will point to a single explicit version of an event type and use these expressions to extract metadata \(`correlationId`/`eventId`\). 

{% hint style="info" %}
To always use the current version of an event type, the version number in a harvester definition can be replaced with the alias `latest`, which will always point to the current version. **Note**: Extraction rules are read on harvester **startup**. So if you update an event type, modified values are not applied to running harvesters even if they are configured to use `latest`. You would need to stop and start the harvester so the extraction rules are re-read.
{% endhint %}

#### Example Call - update event type

```bash
curl -X PUT -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @event-type-update.json \
 https://grnry-host/event-types/snowplow-web
```

{% code title="event-type-update.json" %}
```text
{
  "correlationIdExpression" : "#jsonPath(#jsonPath(payload, 'body'), 'data[1].correlationId')",
  "timestampExpression" : "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[1].timestamp')?:#nowMillis()"
}
```
{% endcode %}

## Deleting an Event Type \(also see [API Docs](../../../developer-reference/api-reference/harvester-api.md#update-an-event-type)\)

Deleting an event type via API call will remove the event type entity and all persister streams \(see [below](event-types.md#eventstore-persister)\) consuming this event type, but only if there are no consuming belts or importing harvesters for this event type known to the system \(deployment state of belts and harvesters is irrelevant\). Otherwise the server will return a 412 status and give information about these belts/harvesters.

#### Example Call - Delete Event Type

```bash
curl -X DELETE -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 https://grnry-host/event-types/snowplow-web
```

## Eventstore Persisters

All imported events are persisted in a database table \(eventstore\). Events of different types might be stored in different eventstores. Therefore there should be at least one persister stream leading to a sink-app per event type. A reasonable pre-configured persister-stream will be created but not started during event type creation. It can be [started](../../../developer-reference/api-reference/harvester-api.md#update-state-of-a-persister-for-a-specific-event-type), [viewed ](../../../developer-reference/api-reference/harvester-api.md#get-persister-for-a-specific-event-type)and [updated ](../../../developer-reference/api-reference/harvester-api.md#update-persister-config-for-a-specific-event-type)via api-calls.

{% hint style="info" %}
The current default implementation is named `pg` and will persist events in a postgres compatible database. It is initialized \(but not started\) automatically on event-type creation, including all setup-specific configuration options \(database name/username/...\). The normal setup workflow of an event-type/harvester does NOT require modifing these settings, they are supposed to work out of the box. 
{% endhint %}

A persister entity has the following attributes:

| Attributes | Description |
| :--- | :--- |
| `eventTypeName` | Name of the event type that is consumed by this persister |
| `eventstoreType` | Type of eventstore to distinguish between multiple eventstores for the same event type. |
| `streamName` | Name of the scdf-stream. Will be generated from `eventTypeName` and `eventstoreType.` |
| `appName` | Name of the sink app. Needs to be registered with scdf beforehand. |
| `appVersion` | Version of the sink app. Needs to be present in scdf. |
| `appConfig` | A map containing app specific configuration. |
| `deployerConfig` | A map containing deployer \(e.g. kubernetes\) specific configuration. |

### Read persister configuration \(also see [API Docs](../../../developer-reference/api-reference/harvester-api.md#get-persister-for-a-specific-event-type)\)

#### Example Call - GET - read persister configuration

```bash
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -o response.json \
 https://grnry-host/event-types/snowplow-web/eventstores/pg/persister
```

{% code title="response.json" %}
```text
{
  "streamName": "g-p-pg-snowplow-web",
  "appName": "grnry-eventstore-pg",
  "appVersion": "latest",
  "appConfig": {
    "eventstore.tablename": "public.eventstore",
    "spring.cloud.kubernetes.secrets.paths": "/usr/src/app/db-secret",
    "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
    "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
    "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
    "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
    "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
    "spring.cloud.stream.kafka.binder.replicationfactor": "3",
    "spring.datasource.password": "${superuser-password}",
    "spring.datasource.url": "jdbc:postgresql://grnry-pg-citus:5432/postgres?currentSchema=public",
    "spring.datasource.username": "${superuser-username}"
  },
  "deployerConfig": {
    "kubernetes.imagepullpolicy": "Always",
    "kubernetes.limits.cpu": "500m",
    "kubernetes.limits.memory": "512Mi",
    "kubernetes.livenessprobedelay": "120",
    "kubernetes.readinessprobedelay": "120",
    "kubernetes.requests.cpu": "500m",
    "kubernetes.requests.memory": "512Mi",
    "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
    "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]"
  },
  "eventTypeName": "test",
  "eventstoreType": "pg",
  "lastUpdated": 1581329715038,
  "_links": {
    "self": {
      "href": "https://grnry-host/event-types/test/eventstores/pg/persister?expand="
    }
  }
}
```
{% endcode %}

### Updating a persister configuration \(also see [API Docs](../../../developer-reference/api-reference/harvester-api.md#update-persister-config-for-a-specific-event-type)\)

#### Example Call - PUT - Updating a Persister

```bash
curl -X PUT -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @persister-update.json \
 https://grnry-host/event-types/snowplow-web/eventstores/pg/persister
```

{% code title="persister-update.json" %}
```text
{
    "appConfig": {
        "eventstore.tablename": "public.eventstore",
        "spring.cloud.kubernetes.secrets.paths": "/usr/src/app/db-secret",
        "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
        "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
        "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
        "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
        "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
        "spring.cloud.stream.kafka.binder.replicationfactor": "3",
        "spring.datasource.password": "${superuser-password}",
        "spring.datasource.url": "jdbc:postgresql://grnry-pg-citus:5432/postgres?currentSchema=public",
        "spring.datasource.username": "${superuser-username}"
    },
    "deployerConfig": {
        "kubernetes.imagepullpolicy": "Always",
        "kubernetes.limits.cpu": "500m",
        "kubernetes.limits.memory": "512Mi",
        "kubernetes.livenessprobedelay": "120",
        "kubernetes.readinessprobedelay": "120",
        "kubernetes.requests.cpu": "500m",
        "kubernetes.requests.memory": "512Mi",
        "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
        "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]"
    }
}
```
{% endcode %}

### Starting / Stopping an persister \(also see [API Docs](../../../developer-reference/api-reference/harvester-api.md#update-state-of-a-persister-for-a-specific-event-type)\)

#### Example Call Start/Stop

```bash
curl -X POST -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @persister-start.json \    # start body
 # -d @persister-stop.json \  # stop body
 https://grnry-host/event-types/snowplow-web/eventstores/pg/persister/state
```

{% code title="persister-start.json" %}
```text
{
  "action" : "START"
}
```
{% endcode %}

{% code title="persister-stop.json" %}
```text
{
  "action" : "STOP"
}
```
{% endcode %}

### Logs Endpoint \(also see [API Docs](../../../developer-reference/api-reference/harvester-api.md#get-persister-logs-for-a-specific-event-type)\)

There is a logs endpoint for running persister streams available. It can be accessed via [API-Call](../../../developer-reference/api-reference/harvester-api.md#get-persister-logs-for-a-specific-event-type). The `lines` request parameter limits the output to the given number of lines.

```bash
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -o persister-logs.json \
 https://grnry-host/event-types/snowplow-web/eventstores/pg/persister/logs?lines=5
```

{% code title="persister-logs.json" %}
```bash
{
    "logs": [
        "\tvalue.serializer = class org.apache.kafka.common.serialization.ByteArraySerializer",
        "",
        "2020-02-17 08:57:40.837  INFO [g-p-pg-test-sink,,,] 1 --- [pool-1-thread-1] o.a.kafka.common.utils.AppInfoParser     : Kafka version : 2.0.1",
        "2020-02-17 08:57:40.838  INFO [g-p-pg-test-sink,,,] 1 --- [pool-1-thread-1] o.a.kafka.common.utils.AppInfoParser     : Kafka commitId : fa14705e51bd2ce5",
        "2020-02-17 08:57:40.841  INFO [g-p-pg-test-sink,,,] 1 --- [ad | producer-2] org.apache.kafka.clients.Metadata        : Cluster ID: v8AY3bE6Sou7aXjAfipNQQ"
    ]
}
```
{% endcode %}

