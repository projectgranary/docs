---
description: In this chapter, we create & deploy all resources to import tracking data.
---

# Registering new Data Pipeline

![](<../../../.gitbook/assets/grafik (27).png>)

{% hint style="info" %}
This tutorial assumes that you have the `editor` role in `global` project. If you want to create the objects in a different project scope, you need to add /projects/{projectName} in front of all your API calls. Exception: Profilestore and Eventstore API calls.
{% endhint %}

## 1. Create Event Type "Customer Session"

In Granary, an event type describe the metadata and schema of a particular category of raw data that you want to import. In our case, this is tracking data consisting of customer sessions. Create your first event type like so:

![](<../../../.gitbook/assets/image (19).png>)

Harvester API Request:

> **POST /event-types/cust-sess-\<yourName>**

`cust-sess-<yourName>` is the immutable name of the event type and the name needs to comply with these [restrictions](../../data-in/best-practices-1/hints-on-naming-of-harvesters-and-event-types.md#event-types).

For the body (look for the tab with the green dot) use this JSON:

```javascript
{
    "displayName": "Customer Session",
    "description": "Event Type for Customer Sessions",
    "correlationIdExpression": "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].correlationId')?:'NO_CORRELATION_ID'",
    "eventIdExpression": "#randomUUID()",
    "timestampExpression": "#nowMillis()"
}
```

Find more detailed explanation on event type configuration [here](../../data-in/how-to-run-a-harvester/event-types.md#creating-an-event-type-also-see-api-docs). Check out the difference between `name` and `displayName` [here](../../../developer-reference/dataflow/event-type.md#difference-between-name-and-displayname).

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "name": "cust-sess-<yourName>",
    "displayName": "Customer Session",
    "version": 1,
    "type": "data_in",
    "editor": "event_type_data_in_edit",
    "consumer": null,
    "topicName": "grnry_data_in_customersession",
    "correlationIdExpression": "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].correlationId')?:'NO_CORRELATION_ID'",
    "eventIdExpression": "#randomUUID()",
    "timestampExpression": "#nowMillis()",
    "retentionMs": 345600000,
    "partitionCount": 32,
    "replication": 2,
    "eventstoreTTL": "P100Y",
    "description": "Event Type for Customer Sessions",
    "_links": {
        "self": {
            "href": "https://<hostname>/event-types/cust-sess-<yourName>/1"
        }
    }
}
```

As you can see, Granary applied a lot of default configuration. That's it for now. We needed to create the event type first so that we can reference in the harvester instance creation below.

## 2. Create Harvester "Snowplow Customer Sessions"

In Granary, a harvester consists of three steps that we need to configure to prepare incoming data to match the event types definition:

1. Source Type
2. Scriptable Transform
3. Metadata Extractor

Most of the harvester configuration is covered by defaults, so creating your first harvester goes like this:

![](<../../../.gitbook/assets/image (5).png>)

Harvester API Request:

> **POST /harvesters/instances/snow-cust-sess-\<yourName>**

`snow-cust-sess-<yourName>` is the immutable name of the harvester and the name needs to comply with these [restrictions](../../data-in/best-practices-1/hints-on-naming-of-harvesters-and-event-types.md#harvesters-1).

For the body (look for the tab with the green dot) use this JSON:

{% hint style="info" %}
The name of the Event Type in line 21 must match the Event Type's `name` you created in the previous step of this chapter.
{% endhint %}

```javascript
{
    "displayName": "Snowplow Customer Sessions",
    "sourceType": {
        "name": "grnry-topic",
        "version": "1.2.0",
    "configuration": {
        "consumer.input-topic": "snowplow"
    }
    },
  "metadataExtractor": {
    "app": "grnry-data-in-metadata",
    "version": "1.2.0"
  },
  "transform": {
    "app": "grnry-scriptable",
    "version": "1.2.0",
    "language": "groovy",
    "script": "import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload\\nimport com.fasterxml.jackson.databind.ObjectMapper\\nimport groovy.json.JsonSlurper\\n\\n// deserialize Snowplow payload\\nSnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)\\n\\n// get normalized JSON String representation of Snowplow event\\neventString = SnowplowPayloadExtractor.getActualSnowplowPayload(snowplowEvent)\\n\\nJsonSlurper slurper = new JsonSlurper()\\n// JSON Object representation of normalized payload\\n\\nevent = slurper.parseText(eventString)\\nif (event?.body?.data?.filterCriteria) {\\n    // find filterCriteria in POST body\\n    if (event.body.data[0].filterCriteria.equalsIgnoreCase(\\\"customer-sessions\\\")) {\\n        return eventString\\n    }\\n}\\n\\n// skip event\\nreturn null"
  },
    "eventType": {
        "name": "cust-sess-<yourName>",
        "version": "latest"
    }
}
```

Find more detailed explanation on harvester configuration [here](../../data-in/how-to-run-a-harvester/harvesters.md).

The script in line 18 of the JSON payload above will filter our events later on and looks actually like this:

```groovy
import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import com.fasterxml.jackson.databind.ObjectMapper
import groovy.json.JsonSlurper

// deserialize Snowplow payload
SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)

// get normalized JSON String representation of Snowplow event
​eventString = SnowplowPayloadExtractor.getActualSnowplowPayload(snowplowEvent)

JsonSlurper slurper = new JsonSlurper()
// JSON Object representation of normalized payload

event = slurper.parseText(eventString)
if (event?.body?.data?.filterCriteria) {
    // find filterCriteria in POST body
    if (event.body.data[0].filterCriteria.equalsIgnoreCase("customer-sessions")) {
        return eventString
    }
}

// skip event
return null
```

Check [this best practice](../../data-in/best-practices-1/easing-development.md) how to transform it to a one-liner.

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "name": "snow-cust-sess-<yourName>",
    "displayName": "Snowplow Customer Sessions",
    "streamName": "g-h-snow-cust-sess-<yourName>",
    "dlqTopic": "grnry_harvester_dlq_snowplow-customer-se",
    "sourceType": {
        "name": "grnry-topic",
        "version": "1.2.0",
        "configuration": {
            "consumer.input-topic": "snowplow"
        },
        "deploymentConfiguration": {
            "kubernetes.imagePullPolicy": "Always",
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.livenessProbeDelay": "120",
            "kubernetes.readinessProbeDelay": "120",
            "kubernetes.requests.cpu": "350m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly: 'true' }]",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]"
        },
        "appConfiguration": {
            "spring.cloud.stream.bindings.output.producer.autoAddPartitions": true,
            "spring.cloud.stream.bindings.output.producer.partitionCount": 12
        },
        "_links": {
            "type": {
                "href": "https://<hostname>/harvesters/source-types/grnry-topic/1.2.0"
            }
        }
    },
    "metadataExtractor": {
        "app": "grnry-data-in-metadata",
        "version": "1.2.0",
        "deploymentConfiguration": {
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.livenessprobedelay": "120",
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.requests.cpu": "350m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]"
        },
        "appConfiguration": {
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
            "spring.cloud.stream.kafka.binder.consumer.maxattempts": "1",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.replicationfactor": "3"
        }
    },
    "transform": {
        "app": "grnry-scriptable",
        "version": "1.2.0",
        "deploymentConfiguration": {
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.livenessprobedelay": "120",
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.requests.cpu": "350m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]"
        },
        "appConfiguration": {
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
            "spring.cloud.stream.kafka.binder.consumer.maxattempts": "1",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.replicationfactor": "3"
        },
        "language": "groovy",
        "script": "import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload\\nimport com.fasterxml.jackson.databind.ObjectMapper\\nimport groovy.json.JsonSlurper\\n\\n// deserialize Snowplow payload\\nSnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)\\n\\n// get normalized JSON String representation of Snowplow event\\neventString = SnowplowPayloadExtractor.getActualSnowplowPayload(snowplowEvent)\\n\\nJsonSlurper slurper = new JsonSlurper()\\n// JSON Object representation of normalized payload\\n\\nevent = slurper.parseText(eventString)\\nif (event?.body?.data?.filterCriteria) {\\n    // find filterCriteria in POST body\\n    if (event.body.data[0].filterCriteria.equalsIgnoreCase(\\\"customer-sessions\\\")) {\\n        return eventString\\n    }\\n}\\n\\n// skip event\\nreturn null"
    },
    "sessionizing": {
        "enabled": false
    },
    "eventType": {
        "name": "cust-sess-<yourName>",
        "version": "latest",
        "_links": {
            "self": {
                "href": "https://<hostname>/event-types/cust-sess-<yourName>/latest"
            }
        }
    },
    "_links": {
        "self": {
            "href": "https://<hostname>/harvesters/instances/snow-cust-sess-<yourName>?expand="
        }
    }
}
```

Again, Granary applied a whole bunch of default configuration. Next up, starting this harvester.

## 3. Start Harvester "Snowplow Customer Sessions"

Within step 2, we created a harvester. In order to be able to consume data, we need to start the harvester. This goes like so:

![](<../../../.gitbook/assets/image (13).png>)

Harvester API request:

> &#x20;**POST /harvesters/instances/snow-cust-sess-\<yourName>/state**

For the body (look for the tab with the green dot) use this JSON:

```javascript
{
    "action": "START"
}
```

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "status": "DEPLOYING",
    "_links": {
        "self": {
            "href": "https://<hostname>/harvesters/instances/snow-cust-sess-<yourName>/state"
        }
    }
}
```

The deployment can take up to three minutes. You can check the current state like so:

![](<../../../.gitbook/assets/image (6).png>)

Harvester API request:

> &#x20;**GET /harvesters/instances/snow-cust-sess-\<yourName>/state**

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "status": "RUNNING",
    "_links": {
        "self": {
            "href": "https://<hostname>/harvesters/instances/snow-cust-sess-<yourName>/state"
        }
    }
}
```

## 4. Start Persister of Event Type "Customer Session"

Within step 2, we created a harvester. In order to be able to persist data in the event store, we need to start the persister of our event type "Customer Session". This goes like so:

For the body (look for the tab with the green dot) use this JSON:

![](<../../../.gitbook/assets/image (10).png>)

Harvester API request:

> POST /event-types/cust-sess-\<yourName>/eventstores/pg/persister/state

```javascript
{
    "action": "START"
}
```

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "status": "DEPLOYING",
    "_links": {
        "self": {
            "href": "https://<hostname>/event-types/cust-sess-<yourName>/eventstores/pg/persister/state"
        }
    }
}
```

The deployment can take up to two minutes. You can check the current state like so:

![](<../../../.gitbook/assets/image (25).png>)

Harvester API request:&#x20;

> **GET /event-types/cust-sess-\<yourName>/eventstores/pg/persister/state**

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "status": "RUNNING",
    "_links": {
        "self": {
            "href": "https://<hostname>/event-types/cust-sess-<yourName>/eventstores/pg/persister/state"
        }
    }
}
```
