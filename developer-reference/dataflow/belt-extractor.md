---
description: A function-as-a-service like Python callback runtime
---

# Belt Extractor

![Data flow within harmonized data zone of Granary](../../.gitbook/assets/dataflow_profile.PNG)

Belts are used to compute updates for the Costumer Graph stored in the Profile Store. These updates can represent things like:

* whether a certain user has reached a goal in a conversion funnel
* what device type or operating system was used for the most recent visit
* when the most recent visit happened
* the increment on a counter that represents the number of visits per week
* a link between an anonymous session profile and an authenticated user
* the addition of a contract the user has and what the value of that contract is
* etc.

They are defined by a label, a scale factor, an input topic and a stateless/serverless Python function that gets invoked for every event received from the respective input topic. The function typically extracts data from the payload in order to compile one or more update statements for the Profile Store. If a function is taking too much time to process a record, it will timeout to prevent errors like infinite loops. The Belt retries and does not recover if the processing time keeps exceeding the timeout. The maximum duration for the function's execution can be configured.

## Input Topics 

Valid input topics for a 

`['grnry_data_in_...', 'grnry_data_in_...', ...]`

## Callback Signature

### Single payload processing 

`execute(event_headers, event_payload[, profile])`

{% tabs %}
{% tab title="Spec" %}
| Key | Description |
| :--- | :--- |
| **event\_headers** | Kafka fields for Event metadata |
| $$-$$ grnry-event-type | event type as specified during harvester definition |
| $$-$$ grnry-event-id | used to deduplicate events |
| $$-$$ grnry-harvester-name | name of the harvester instance, extracted from event payload or a static value |
| $$-$$ grnry-correlation-id | used to group events received from the same tracking entity |
| $$-$$ grnry-event-timestamp | event processing time set by harvester \(metadata extractor\) |
| $$-$$ grnry-event-type-version | version of event type registered with the harvester |
| **event\_payload** | Forwarded from input attribute `value`  |
| $$-$$ schema | Snowplow Event Schema Reference |
| $$-$$ ipAddress | ipAddress if Snowplow is configured to collect this |
| $$-$$ timestamp | time of event creation or reception\(?\) |
| $$-$$ collector | identifies the source platform of the event |
| $$-$$ body | Snowplow Event Data \(according to `schema`\) |
| $$-$$ headers | HTTP headers |
| **profile** | A profile fetched from [profile store](profile-store/). Only provided if `fetch profile` in [belt definition](../api-reference/belt-api.md) is set to`true.` |
{% endtab %}

{% tab title="Example" %}
```python
event_headers = {
			"grnry-event-type":"...",
			"grnry-event-id":"...",
			"grnry-correlation-id":"...",
			"grnry-harvester-name":"...",
			"grnry-event-timestamp":1535972952300
		}
event_payload = {
		"body": "{ ... }",
		"collector": "ssc-0.13.0-kafka",
		"encoding": "UTF-8",
		"headers": ["Host: aws-eu1.grnry.io", "Accept: text/html, application/xhtml+xml, application/xml;q=0.9, image/webp, image/apng, */*;q=0.8", "Accept-Encoding: gzip, deflate, br", "Accept-Language: de-DE, de;q=0.9, en-US;q=0.8, en;q=0.7", "Cookie: _ga=GA1.2.1323636424.1533625971; ajs_anonymous_id=%22058bbd8d-cb74-47e4-aa27-9cc5fa4546aa%22; ajs_group_id=null; ajs_user_id=%22QjAVZpXa7qgJdZs6vGsNMA5M9yH3%22; mp_96b84420a1a32e448f73e7b9ffccebdb_mixpanel=%7B%22distinct_id%22%3A%20%22165133b330da0-0ad91354656274-47e1039-1fa400-165133b330f15e%22%2C%22%24initial_referrer%22%3A%20%22%24direct%22%2C%22%24initial_referring_domain%22%3A%20%22%24direct%22%7D", "Upgrade-Insecure-Requests: 1", "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36", "X-Forwarded-For: 82.207.192.113", "X-Forwarded-Port: 443", "X-Forwarded-Proto: https", "Connection: keep-alive", "Timeout-Access: <function1>"],
		"hostname": "aws-eu1.grnry.io",
		"ipAddress": "82.207.192.113",
		"networkUserId": "631b9979-16b8-44ed-87b1-86b7d92d8223",
		"path": "/i",
		"schema": "iglu:com.snowplowanalytics.snowplow/CollectorPayload/thrift/1-0-0",
		"timestamp": 1535972952029,
		"userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36"
	}
	
profile = {
  "_id": "0815",
  "a1": {
    "_latest": {
      "_v": "abc",
      "_c": 0.4,
      "_in": 123,
      "_ttl": "P100Y",
      "_origin": null,
      "_reader": "_all"
    }
  },
  "a2": {
    "b1": {
      "2018-09-21": {
        "_v": "21",
        "_c": 0.1,
        "_ttl": "P100Y",
        "_origin": null,
        "_reader": "_all"
      },
      "2018-09-22": {
        "_v": "22",
        "_c": 0.2,
        "_ttl": "P100Y",
        "_origin": null,
        "_reader": "_all"
      },
      "_latest": {
        "_v": "23",
        "_in": "2018-09-23",
        "_c": 0.3,
        "_ttl": "P100Y",
        "_origin": null,
        "_reader": "_all"
      }
    },
    "b2": {
      "_latest": {
        "_v": "123456",
        "_c": 0.4,
        "_in": "2018-09-20",
        "_ttl": "P100Y",
        "_origin": null,
        "_reader": "_all"
      }
    }
  },
  "a3": {
    "b3": {
      "c3": {
        "d3": {
          "e3": {
            "_latest": {
              "_v": "123456",
              "_c": 0.5,
              "_in": "2018-09-20",
              "_ttl": "P100Y",
              "_origin": null,
              "_reader": "_all"
            }
          }
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Multiple payloads/batch data processing

`execute(event_headers, event_payload[, profile])`

Similar to the single payload approach, the callback signature accepted by the belt framework consists of event\_headers and event\_payload, as well as profiles in case profile fetching is set to true. The only difference is that in this case both event\_headers and event\_payload will both be a _list_ of header and payload _dictionaries_, respectively, looking for example like this:

#### Headers

```yaml
[
   {
      "grnry-harvester-name":"snowplow-a-std-harvester",
      "grnry-event-type":"snowplow-a",
      "grnry-event-id":"8be4a767-7ee0-4bb0-addd-6eab0c6e1b22",
      "grnry-correlation-id":"23456",
      "grnry-event-timestamp":"1574009867086",
      "grnry-event-type-version":"1"

   },
   {
      "grnry-harvester-name":"snowplow-a-std-harvester",
      "grnry-event-type":"snowplow-a",
      "grnry-event-id":"b785b078-e0ef-408f-b77c-2794f1e3a6ea",
      "grnry-correlation-id":"23456",
      "grnry-event-timestamp":"1574009868613",
      "grnry-event-type-version":"1"
   },
   {
      "grnry-harvester-name":"snowplow-a-std-harvester",
      "grnry-event-type":"snowplow-a",
      "grnry-event-id":"f388c58b-5eb9-4507-898b-023d42e01033",
      "grnry-correlation-id":"23456",
      "grnry-event-timestamp":"1574009869741",
      "grnry-event-type-version":"1"
   },
 ...
]
```

#### Payloads

```python
[
	'{
		"setNetworkUserId":true,
		"headersIterator":[
			"Host: hostname",
			"X-Request-ID: 438e8",
			"X-Real-Ip: 127.0.0.1",
			"X-Forwarded-For: 127.0.0.1",
			"X-Forwarded-Host: hostname",
			"X-Forwarded-Port: 443",
			"X-Forwarded-Proto: https",
			"X-Original-URI: /api/com.snowplowanalytics.snowplow/tp2",
			"X-Scheme: https",
			"X-B3-TraceId: 5f910",
			"X-B3-SpanId: 5f910",
			"Authorization: Bearer eyJhb",
			"User-Agent: PostmanRuntime/7.17.1",
			"Accept: */*",
			"Cache-Control: no-cache",
			"Postman-Token: 3e458",
			"Accept-Encoding: gzip, deflate",
			"Timeout-Access: <function1>",
			"application/json"
		],
		"encoding":"UTF-8",
		"setBody":true,
		"headersSize":19,
		"setQuerystring":false,
		"setPath":true,
		"refererUri":null,
		"timestamp":1574009866075,
		"path":"/com.snowplowanalytics.snowplow/tp2",
		"setSchema":true,
		"body":"{\\n  \\"data\\": [\\n    {\\n      \\"correlationId\\": \\"23456\\",\\n      \\"filterCriteria\\": \\"a\\"\\n    }\\n  ],\\n  \\"simple_pairs\\": [\\n    {\\n      \\"cid\\": \\"23456\\",\\n      \\"type\\": \\"_d\\",\\n      \\"value\\": \\"hallo\\",\\n      \\"reader\\": \\"_auth\\",\\n      \\"path\\": \\"contract/VERTRAG-4\\",\\n      \\"origin\\":\\"belt\\"\\n    }\\n  ]\\n}",
		"ipAddress":"127.0.0.1",
		"schema":"iglu:com.snowplowanalytics.snowplow/CollectorPayload/thrift/1-0-0",
		"setHostname":true,
		"setUserAgent":true,
		"setTimestamp":true,
		"setEncoding":true,
		"setCollector":true,
		"setRefererUri":false,
		"hostname":"hostname",
		"setHeaders":true,
		"querystring":null,
		"headers":[
			"Host: hostname",
			"X-Request-ID: 438e8",
			"X-Real-Ip: 127.0.0.1",
			"X-Forwarded-For: 127.0.0.1"
			"X-Forwarded-Host: hostname",
			"X-Forwarded-Port: 443",
			"X-Forwarded-Proto: https",
			"X-Original-URI: /api/com.snowplowanalytics.snowplow/tp2",
			"X-Scheme: https",
			"X-B3-TraceId: 5f910",
			"X-B3-SpanId: 5f910",
			"Authorization: Bearer eyJhb",
			"User-Agent: PostmanRuntime/7.17.1",
			"Accept: */*",
			"Cache-Control: no-cache",
			"Postman-Token: 3e458",
			"Accept-Encoding: gzip, deflate",
			"Timeout-Access: <function1>",
			"application/json"
		],
		"contentType":"application/json",
		"setIpAddress":true,
		"userAgent":"PostmanRuntime/7.17.1",
		"setContentType":true,
		"collector":"ssc-0.15.0-kafka",
		"networkUserId":"4770e"
	}',
	'{
		"setNetworkUserId":true,
		"headersIterator":[
			"Host: hostname",
			"X-Request-ID: e091d",
			"X-Real-Ip: 127.0.0.1",
			"X-Forwarded-For: 127.0.0.1",
			"X-Forwarded-Host: hostname",
			"X-Forwarded-Port: 443",
			"X-Forwarded-Proto: https",
			"X-Original-URI: /api/com.snowplowanalytics.snowplow/tp2",
			"X-Scheme: https",
			"X-B3-TraceId: 5f910",
			"X-B3-SpanId: 5f910",
			"Authorization: Bearer eyJhb",
			"User-Agent: PostmanRuntime/7.17.1",
			"Accept: */*",
			"Cache-Control: no-cache",
			"Postman-Token: f0927",
			"Accept-Encoding: gzip, deflate",
			"Timeout-Access: <function1>",
			"application/json"
		],
		"encoding":"UTF-8",
		"setBody":true,
		"headersSize":19,
		"setQuerystring":false,
		"setPath":true,
		"refererUri":null,
		"timestamp":1574009867606,
		"path":"/com.snowplowanalytics.snowplow/tp2",
		"setSchema":true,
		"body":"{\\n  \\"data\\": [\\n    {\\n      \\"correlationId\\": \\"23456\\",\\n      \\"filterCriteria\\": \\"a\\"\\n    }\\n  ],\\n  \\"simple_pairs\\": [\\n    {\\n      \\"cid\\": \\"23456\\",\\n      \\"type\\": \\"_d\\",\\n      \\"value\\": \\"hallo\\",\\n      \\"reader\\": \\"_auth\\",\\n      \\"path\\": \\"contract/VERTRAG-4\\",\\n      \\"origin\\":\\"belt\\"\\n    }\\n  ]\\n}",
		"ipAddress":"127.0.0.1",
		"schema":"iglu:com.snowplowanalytics.snowplow/CollectorPayload/thrift/1-0-0",
		"setHostname":true,
		"setUserAgent":true,
		"setTimestamp":true,
		"setEncoding":true,
		"setCollector":true,
		"setRefererUri":false,
		"hostname":"hostname",
		"setHeaders":true,
		"querystring":null,
		"headers":[
			"Host: hostname",
			"X-Request-ID: e091d",
			"X-Real-Ip: 127.0.0.1",
			"X-Forwarded-For: 127.0.0.1",
			"X-Forwarded-Host: hostname",
			"X-Forwarded-Port: 443",
			"X-Forwarded-Proto: https",
			"X-Original-URI: /api/com.snowplowanalytics.snowplow/tp2",
			"X-Scheme: https",
			"X-B3-TraceId: 5f910",
			"X-B3-SpanId: 5f910",
			"Authorization: Bearer eyJhb",
			"User-Agent: PostmanRuntime/7.17.1",
			"Accept: */*",
			"Cache-Control: no-cache",
			"Postman-Token: f0927",
			"Accept-Encoding: gzip, deflate",
			"Timeout-Access: <function1>",
			"application/json"
		],
		"contentType":"application/json",
		"setIpAddress":true,
		"userAgent":"PostmanRuntime/7.17.1",
		"setContentType":true,
		"collector":"ssc-0.15.0-kafka",
		"networkUserId":"0e428"
	}', 
	'{
		"setNetworkUserId":true,
		"headersIterator":[
			"Host: hostname",
			"X-Request-ID: 70b54",
			"X-Real-Ip: 127.0.0.1",
			"X-Forwarded-For: 127.0.0.1",
			"X-Forwarded-Host: hostname",
			"X-Forwarded-Port: 443",
			"X-Forwarded-Proto: https",
			"X-Original-URI: /api/com.snowplowanalytics.snowplow/tp2",
			"X-Scheme: https",
			"X-B3-TraceId: 5f910",
			"X-B3-SpanId: 5f910",
			"Authorization: Bearer eyJhb",
			"User-Agent: PostmanRuntime/7.17.1",
			"Accept: */*",
			"Cache-Control: no-cache",
			"Postman-Token: c209b",
			"Accept-Encoding: gzip, deflate",
			"Timeout-Access: <function1>",
			"application/json"
		],
		"encoding":"UTF-8",
		"setBody":true,
		"headersSize":19,
		"setQuerystring":false,
		"setPath":true,
		"refererUri":null,
		"timestamp":1574009868734,
		"path":"/com.snowplowanalytics.snowplow/tp2",
		"setSchema":true,
		"body":"{\\n  \\"data\\": [\\n    {\\n      \\"correlationId\\": \\"23456\\",\\n      \\"filterCriteria\\": \\"a\\"\\n    }\\n  ],\\n  \\"simple_pairs\\": [\\n    {\\n      \\"cid\\": \\"23456\\",\\n      \\"type\\": \\"_d\\",\\n      \\"value\\": \\"hallo\\",\\n      \\"reader\\": \\"_auth\\",\\n      \\"path\\": \\"contract/VERTRAG-4\\",\\n      \\"origin\\":\\"belt\\"\\n    }\\n  ]\\n}",
		"ipAddress":"127.0.0.1",
		"schema":"iglu:com.snowplowanalytics.snowplow/CollectorPayload/thrift/1-0-0",
		"setHostname":true,
		"setUserAgent":true,
		"setTimestamp":true,
		"setEncoding":true,
		"setCollector":true,
		"setRefererUri":false,
		"hostname":"hostname",
		"setHeaders":true,
		"querystring":null,
		"headers":[
			"Host: hostname",
			"X-Request-ID: 70b54",
			"X-Real-Ip: 127.0.0.1",
			"X-Forwarded-For: 127.0.0.1",
			"X-Forwarded-Host: hostname",
			"X-Forwarded-Port: 443",
			"X-Forwarded-Proto: https",
			"X-Original-URI: /api/com.snowplowanalytics.snowplow/tp2",
			"X-Scheme: https",
			"X-B3-TraceId: 5f910",
			"X-B3-SpanId: 5f910",
			"Authorization: Bearer eyJhb",
			"User-Agent: PostmanRuntime/7.17.1",
			"Accept: */*",
			"Cache-Control: no-cache",
			"Postman-Token: c209b",
			"Accept-Encoding: gzip, deflate",
			"Timeout-Access: <function1>",
			"application/json"
		],
		"contentType":"application/json",
		"setIpAddress":true,
		"userAgent":"PostmanRuntime/7.17.1",
		"setContentType":true,
		"collector":"ssc-0.15.0-kafka",
		"networkUserId":"f4aa0"
	}', 
... 
]
```

### Callback example for multiple payloads

Given multiple requests with the following body:

```yaml
{
  "data": [
    {
      "correlationId": "23456",
      "filterCriteria": "a"
    }
  ],
  "simple_pairs": [
    {
      "cid": "23456",
      "type": "_d",
      "value": "hallo",
      "reader": "_auth",
      "path": "contract/VERTRAG-4",
      "origin": "/belts/123"
    }
  ]
}
```

the belt extractor consuming from a batch Kafka topic can be equipped with the following Python callback function \(see Configuration section\):

```yaml
import json
import sys
from time import time
from grnry.beltextractor.update 
import Update
import logging
import random

def lookup(dictionary,keys):
   if type(dictionary)==type(''):
            dictionary=json.loads(dictionary)
                try:
                    if len(keys)>1:
                       value = lookup(dictionary[keys[0]],keys[1:])
                    else:
                       value = dictionary[keys[0]]
                       return value    
                 except:        
                    return None
 
def makepath(stringpath):    
   path=stringpath.split('/')
   return path

def create_update(correlation_id, path, value):
   update=Update(correlation_id, path, operation='_array_append');
   update.set_value(value=value, origin='integration-test', reader='_all')
   update.set_type('toe-436-test')
   return update
   
def execute(event_headers, event_payload, profile=None):
    if not event_headers:
      return None
    zipped_events = zip(event_headers, event_payload)    
    updates = [create_update(event[0]['grnry-correlation-id'], makepath(json.loads(json.loads(event[1])['body'])['simple_pairs'][0]['path']), json.loads(json.loads(event[1])['body'])['simple_pairs'][0]['value']) for event in zipped_events]
    logging.debug(updates)
    logging.debug(updates[0]())
    return updates
```

### Callback example for mixed payload processing

`execute(event_headers, event_payload[, profile])`

There are use cases where you want to process events from a topic that contains both single-payload events and multi-payload events. To do so it is necessary to differentiate the two types of events in the callback function as such:



```yaml
import json
import sys
from time import time
from grnry.beltextractor.update 
import Update
import logging


def execute(event_headers, event_payload, profile=None):
    if not event_headers:
      return None
      
 if isinstance(event_headers, dict):
      logging.debug("received single-payload event")
      # add your code handling single-payload events here 
    elif isinstance(event_headers, list) and isinstance(event_payload, list):     
      logging.debug("received multi-payload event")
      # add your code handling multi-payload events here
    else:
      raise ValueError("event_headers and/or event_payload do not match the expected signature") 
```

## Configuration

{% tabs %}
{% tab title="Spec" %}
#### Input Topic

The name of the Kafka topic this Belt will receive events from. Topics are typically designated by event type, so the consuming belt can assume a fixed schema.

#### Python callback function

The Python code provided here has to be in the form of a function by the name **callback.py** that can be invoked using the signature `execute(event_headers, event_payload)`where the parameter `event_payload` and `event_headers`are both a Python dictionary containing one event/header from the input topic or a list of dictionaries containing multiple events/headers.

The function can return either `None` , `[]`  or a list of [Update ](https://gitlab.alvary.io/grnry/belt-extractor/blob/master/grnry/beltextractor/update.py)objects representing an update to a profile in the profile store. If the return is not an Update object the belt will continue with the next message. In case it is, the object needs to be following below schema:

```text
UPDATE :=
  {
    "_schema": string,                                         # schema of upddate message, default is null
    "_operation": a valid update operation,                    # default is "_set"
    "_id": string,
    "_path": \[ string [,string]* \],                          # array of length >= 1
    "_value": GRAIN_VALUE,                                     # must not be null
    "_profile_type": string                                    # default is "_d"
  }

```

Whereas valid update operations can be found [here](profile-store/#update-operations) and a valid GRAIN\_VALUE  __is defined as follows:

```text
GRAIN_VALUE := 
  {
    "_v": string,                                    # Json object string, Json array string, or string of counter value. Value type must fit _operation
    "_c": double,                                    # default is 1
    "_in": long,                                     # default is now()
    "_ttl": string,                                  # period of time, https://en.wikipedia.org/wiki/ISO_8601#Durations, default is "P100Y"
    "_origin": string,                               # default is "/belts/{belt-id}"
    "_reader": string                                # default is "_all"
  }
```

Based on the update object generated by the callback, one ore more JSON update\(s\) will be written to the update topic. Every update object returned by this function must have **at least** the following attribute set: 

| Key | Method | Description |
| :--- | :--- | :--- |
| `_id` | `Update(id, [path])` | Specifies the profile that should be updated. \(part of constructor\) |
| `_path` | `Update(id, [path])` | The path within the nested structure of a profile that should be updated. In case the path doesn't exist yet it will be created. \(part of constructor\) |
| `_value` | `set_value(value, certainty, _in, ttl, origin, reader)` | Contains the value that should be set in the profile under the defined \_path. `set_value()` expects the actual value `_v` and optional `_c`, `_in`, `_ttl`, `_origin`, `_reader`_._ **Example** of a **default grain** value with only __`_v` set:`{ "_v": _v, "_c": 1.0, "_in": time(), "_ttl": "P100Y", "_origin": "/belts/{belt-id}", "_reader": "_auth"` |

If unspecified, default values will be used:

| Attribute Name | Method | Default |
| :--- | :--- | :--- |
| `_schema` | `set_schema(schema)` | `null` |
| `_operation` | `set_operation(operation)` | `"_set"` |
| `_profile_type` | `set_type(profile_type)` | `"_d"` |

**Callback Timeout**

As described above, a function will timeout if excecution takes too long. Another time limit monitors how often long-running callbacks occur. The latter limit should be smaller than the function timeout. Both timeouts can be configured using environment variables in Belt Extractor via the [Belt API](../api-reference/belt-api.md#create-and-store-a-belt)'s parameter `extraEnvs`:

| Environment Variable | Default |
| :--- | :--- |
| CALLBACK\_TIMEOUT\_SEC | 300 |
| CALLBACK\_LONGRUNNING\_SEC | 180 |
{% endtab %}

{% tab title="Example" %}
```python
from time import time
from grnry.beltextractor.update import Update

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
{% endtab %}
{% endtabs %}



## Dead letter queue

In GRNRY, we have created so called _dead letter queues_. The Belt Extractor's dead letter queue is used to receive all the data that could not be processed correctly.

By default the Belt Extractor's dead letter queue is called: 

```yaml
belts_dead_letter
```

In order to rename it one must change the helm deployment parameter:

```yaml
extraEnv:
  - name: KAFKA_ERROR_TOPIC
    value: belts_dead_letter
```

#### When is something written to the Dead Letter Queue?

The Belt Extractor writes events to dead letter queue in case of exceptions are thrown during event processing within the belt framework, e.g. if an error occurs during the en-/decryption of messages. Exceptions thrown within the callback function are not written into dead letter queue. Such errors will only be logged and the exception counter is increased.

## Output Topic `'profile-update'`

{% tabs %}
{% tab title="Spec" %}
see [https://gitlab.alvary.io/grnry/kafka-profile-update/blob/master/PROFILESPECS.md](https://gitlab.alvary.io/grnry/kafka-profile-update/blob/master/PROFILESPECS.md)

| Key | Description |
| :--- | :--- |
| `_schema` | schema of update message |
| `_operation` | update operation, default is `_set`, see [Profile Store](profile-store/#component-profile-updater) for more information |
| `_id` | identifies the profile that should be updated with this message |
| `_path` | The path within the nested structure of a profile that should be updated. In case the path doesn't exist yet it will be created. An array of length &gt;= 1 |
| `_value` | _Mandatory_. The grain value that should be set in the profile under the defined `_path` _._ Needs to be set by `set_value()`. For `_delete` operation `_v`is used to specify an array of pits to be deleted from that path. `""`, `[""]` or `[]` will delete `_latest`. |
| `_profile_type` | Categorizes the profile to be updated. Default is `_d`. |
{% endtab %}

{% tab title="Example" %}
```javascript
{
  "_schema": null,
  "_operation": "_set_with_history",
  "_id": "007",
  "_profile_type": "_d",
  "_path": [
    "a2",
    "b1"
  ],
  "_value": {
    "_v": "22",
    "_in": "2018-09-24",
    "_c": 0.4,
    "_ttl": "P3M",
    "_origin": "/belts/123",
  }
}

```
{% endtab %}
{% endtabs %}



