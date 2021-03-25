---
description: Examples for the signature of the callback's mandatory execute() method
---

# Belt Callback Signatures

## Single payload processing

`execute(event_headers, event_payload[, profile])`

where `profile` is a profile fetched from [Profile Store](../../../developer-reference/dataflow/profile-store/). This is only provided if `fetchProfile` in [Belt definition](../../../developer-reference/api-reference/belt-api.md#create-and-store-a-belt) is set to `true`.

#### Event Headers

```yaml
{
  "grnry-event-type": "...",
  "grnry-event-id": "...",
  "grnry-correlation-id": "23456",
  "grnry-harvester-name": "...",
  "grnry-event-timestamp": "1535972952300",
  "grnry-retry-count": "0",
  "grnry-retry-max-count": "4"
}
```

#### Event Payload

```yaml
{
  "setNetworkUserId": true,
  "headersIterator": [
    "Host: development.external.analytics.cc.syncier.cloud",
    "X-Request-ID: 2b43747d9b2464f1c61141521e6a6a1d",
    "X-Real-Ip: 77.20.35.246",
    "X-Forwarded-For: 77.20.35.246",
    "X-Forwarded-Host: development.external.analytics.cc.syncier.cloud",
    "X-Forwarded-Port: 443",
    "X-Forwarded-Proto: https",
    "X-Scheme: https",
    "User-Agent: PostmanRuntime/7.26.8",
    "Accept: */*",
    "Postman-Token: a5c20c2c-d975-4a28-b503-a101514be50e",
    "Accept-Encoding: gzip, deflate, br",
    "Timeout-Access: <function1>",
    "application/json"
  ],
  "encoding": "UTF-8",
  "setBody": true,
  "headersSize": 14,
  "setQuerystring": false,
  "setPath": true,
  "refererUri": null,
  "timestamp": 1607429014845,
  "path": "/com.snowplowanalytics.snowplow/tp2",
  "setSchema": true,
  "body": "{\n    \"data\": [\n\t{\n\t  \"correlationId\": \"23456\",\n\t  \"filterCriteria\": \"a\"\n\t},\n    \"retry\"\n ]\n}",
  "ipAddress": "77.20.35.246",
  "schema": "iglu:com.snowplowanalytics.snowplow/CollectorPayload/thrift/1-0-0",
  "setHostname": true,
  "setUserAgent": true,
  "setTimestamp": true,
  "setEncoding": true,
  "setCollector": true,
  "setRefererUri": false,
  "hostname": "{\\n  \\"data\\": [\\n    {\\n      \\"correlationId\\": \\"23456\\",\\n      \\"filterCriteria\\": \\"a\\"\\n    }\\n  ],\\n  \\"simple_pairs\\": [\\n    {\\n      \\"type\\": \\"_d\\",\\n      \\"value\\": \\"hallo\\",\\n      \\"reader\\": \\"_auth\\",\\n      \\"path\\": \\"grain1\\"\\n    }\\n  ]\\n}"
  "setHeaders": true,
  "querystring": null,
  "headers": [
    "Host: development.external.analytics.cc.syncier.cloud",
    "X-Request-ID: 2b43747d9b2464f1c61141521e6a6a1d",
    "X-Real-Ip: 77.20.35.246",
    "X-Forwarded-For: 77.20.35.246",
    "X-Forwarded-Host: development.external.analytics.cc.syncier.cloud",
    "X-Forwarded-Port: 443",
    "X-Forwarded-Proto: https",
    "X-Scheme: https",
    "User-Agent: PostmanRuntime/7.26.8",
    "Accept: */*",
    "Postman-Token: a5c20c2c-d975-4a28-b503-a101514be50e",
    "Accept-Encoding: gzip, deflate, br",
    "Timeout-Access: <function1>",
    "application/json"
  ],
  "contentType": "application/json",
  "setIpAddress": true,
  "userAgent": "PostmanRuntime/7.26.8",
  "setContentType": true,
  "collector": "ssc-0.15.0-kafka",
  "networkUserId": "14d9405a-b6eb-491f-a820-c675c655136a"
}
```

#### Profile

```yaml
{
  "correlationId": "23456",
  "type": "_d",
  "jsonPayload": {
    "grain-1": {
      "_latest": {
        "_c": 1,
        "_v": "hallo-1",
        "_in": 1607348646916,
        "_ttl": "P100Y",
        "_origin": "/belts/33",
        "_reader": "_auth"
      }
    },
    "grain-2": {
      "_latest": {
        "_c": 1,
        "_v": "hello-2",
        "_in": 1607348647930,
        "_ttl": "P100Y",
        "_origin": "/belts/33",
        "_reader": "_all"
      }
    },
    "grain-3": {
      "_latest": {
        "_c": 1,
        "_v": "hello-3",
        "_in": 1607348648942,
        "_ttl": "P100Y",
        "_origin": "/belts/33",
        "_reader": "_all"
      }
    },
    "grain-4": {
      "_latest": {
        "_c": 1,
        "_v": "hello-5",
        "_in": 1607348649951,
        "_ttl": "P100Y",
        "_origin": "/belts/33",
        "_reader": "_all"
      }
    },
    "grain-5": {
      "_latest": {
        "_c": 1,
        "_v": "hello-6",
        "_in": 1607348650964,
        "_ttl": "P100Y",
        "_origin": "/belts/33",
        "_reader": "_all"
      }
    },
    "_id": "23456"
  }
}
```

### Callback example for single payload

Given a single request with the following body:

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
      "type": "_d",
      "value": "hallo",
      "reader": "_auth",
      "path": "grain-1"
    }
  ]
}
```

The belt extractor consuming from a batch Kafka topic can be equipped with the following Python callback function \(see Configuration section\):

{% code title="callback.py" %}
```python
import json

def execute(event_headers, event, profile=None):

    correlation_id = event_headers['grnry-correlation-id']
    event_data = lookup(event, ['body'])

    if event_data is None:
        return None

    updates = []

    if lookup(event_data, ['integration_test_data']):

        for simple_pair in lookup(event_data, ['simple_pairs']):
            value = simple_pair['value']
            path = makepath(simple_pair['path'])
            reader = simple_pair['reader'] if simple_pair['reader'] else '_all'
            updates.append(Update(correlation_id, path))
            updates[-1].set_value(value=value, reader=reader)
            if lookup(simple_pair, ['operation']):
                operation = simple_pair['operation']                
                updates[-1].set_operation(operation)
            if lookup(simple_pair, ['type']):
                type = simple_pair['type']
                updates[-1].set_type(type)
    return updates


def lookup(dictionary, keys):
    if type(dictionary) == type(''):
        dictionary = json.loads(dictionary)
    try:
        if len(keys) > 1:
            value = lookup(dictionary[keys[0]], keys[1:])
        else:
            value = dictionary[keys[0]]
        return value
    except:
        return None


def makepath(stringpath):
    path = stringpath.split('/')
    return path
```
{% endcode %}

## Multiple payloads/batch data processing

`execute(event_headers, event_payload[, profile])`

If [sessionizing](../../data-in/best-practices-1/sessionize-data.md) is used, the callback signature accepted by the belt framework consists of event\_headers and event\_payload, as well as profiles in case profile fetching is set to true. The only difference is that in this case both event\_headers and event\_payload will both be a _list_ of header and payload _dictionaries_, respectively, looking for example like this:

#### Event Headers

```yaml
[
   {
      "grnry-harvester-name":"snowplow-a-std-harvester",
      "grnry-event-type":"snowplow-a",
      "grnry-event-id":"8be4a767-7ee0-4bb0-addd-6eab0c6e1b22",
      "grnry-correlation-id":"23456",
      "grnry-event-timestamp":"1574009867086",
      "grnry-event-type-version":"1"
      ...
   },
   {
      "grnry-harvester-name":"snowplow-a-std-harvester",
      "grnry-event-type":"snowplow-a",
      "grnry-event-id":"b785b078-e0ef-408f-b77c-2794f1e3a6ea",
      "grnry-correlation-id":"23456",
      "grnry-event-timestamp":"1574009868613",
      "grnry-event-type-version":"1"
      ...
   },
   {
      "grnry-harvester-name":"snowplow-a-std-harvester",
      "grnry-event-type":"snowplow-a",
      "grnry-event-id":"f388c58b-5eb9-4507-898b-023d42e01033",
      "grnry-correlation-id":"23456",
      "grnry-event-timestamp":"1574009869741",
      "grnry-event-type-version":"1"
      ...
   },
 ...
]
```

#### Event Payloads

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

{% code title="callback.py" %}
```python
import json
import sys
from time import time
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
{% endcode %}

### Callback example for mixed payload processing

There are use cases where you want to process events from a topic that contains both single-payload events and multi-payload events. To do so it is necessary to differentiate the two types of events in the callback function as such:

{% code title="callback.py" %}
```python
import json
import sys
from time import time
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
{% endcode %}

## 

