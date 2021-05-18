---
description: 'In this chapter, we create & deploy all resources to import tracking data.'
---

# Registering new Data Pipeline

![](../../../.gitbook/assets/grafik%20%2820%29%20%281%29.png)

## 1. Create Event Type "Customer Session"

In Granary, an event type describe the metadata and schema of a particular category of raw data that you want to import. In our case, this is tracking data consisting of customer sessions. Create your first event type like so:

![Harvester API Request: POST/event-types](../../../.gitbook/assets/image%20%2819%29%20%281%29.png)

For the body \(look for the tab with the green dot\) use this JSON:

```javascript
{
	"displayName": "Customer Session",
	"description": "Event Type for Customer Sessions",
	"correlationIdExpression": "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].correlationId')?:'NO_CORRELATION_ID'",
	"eventIdExpression": "#randomUUID()",
	"timestampExpression": "#nowMillis()"
}
```

Find more detailed explanation on event type configuration [here](../../data-in/how-to-run-a-harvester/event-types.md#creating-an-event-type-also-see-api-docs).

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "name": "customer-session",
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
            "href": "https://<hostname>/event-types/customersession/1"
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

![Harvester API Request: POST /harvesters/instances](../../../.gitbook/assets/image%20%285%29.png)

For the body \(look for the tab with the green dot\) use this JSON:

{% hint style="info" %}
The name of the Event Type in line 21 must match the Event Type's `name` you created in the previous step of this chapter.
{% endhint %}

```javascript
{
	"displayName": "Snowplow Customer Sessions",
	"sourceType": {
		"name": "grnry-topic",
		"version": "0.8.1",
    "configuration": {
        "consumer.input-topic": "snowplow"
    }
	},
  "metadataExtractor": {
    "app": "grnry-data-in-metadata",
    "version": "0.8.1"
  },
  "transform": {
    "app": "grnry-scriptable",
    "version": "0.8.1",
    "language": "groovy",
    "script": "import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload\\nimport com.fasterxml.jackson.databind.ObjectMapper\\nimport groovy.json.JsonSlurper\\n\\n// deserialize Snowplow payload\\nSnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)\\n\\n// get normalized JSON String representation of Snowplow event\\neventString = SnowplowPayloadExtractor.getActualSnowplowPayload(snowplowEvent)\\n\\nJsonSlurper slurper = new JsonSlurper()\\n// JSON Object representation of normalized payload\\n\\nevent = slurper.parseText(eventString)\\nif (event?.body?.data?.filterCriteria) {\\n    // find filterCriteria in POST body\\n    if (event.body.data[0].filterCriteria.equalsIgnoreCase(\\\"customer-sessions\\\")) {\\n        return eventString\\n    }\\n}\\n\\n// skip event\\nreturn null"
  },
	"eventType": {
		"name": "customer-session",
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
    "name": "snowplow-customer-se",
    "displayName": "Snowplow Customer Sessions",
    "streamName": "g-h-snowplow-customer-se",
    "dlqTopic": "grnry_harvester_dlq_snowplow-customer-se",
    "sourceType": {
        "name": "grnry-topic",
        "version": "0.8.1",
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
                "href": "https://<hostname>/harvesters/source-types/grnry-topic/0.8.0"
            }
        }
    },
    "metadataExtractor": {
        "app": "grnry-data-in-metadata",
        "version": "0.8.1",
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
        "version": "0.8.1",
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
        "name": "customer-session",
        "version": "latest",
        "_links": {
            "self": {
                "href": "https://<hostname>/event-types/customersession/latest"
            }
        }
    },
    "_links": {
        "self": {
            "href": "https://<hostname>/harvesters/instances/snowplow-customer-se?expand="
        }
    }
}
```

Again, Granary applied a whole bunch of default configuration. Next up, starting this harvester.

## 3. Start Harvester "Snowplow Customer Sessions"

Within step 2, we created a harvester. In order to be able to consume data, we need to start the harvester. This goes like so:

![Harvester API: POST /harvesters/instances/snowplow-customer-se/state](../../../.gitbook/assets/image%20%2813%29.png)

For the body \(look for the tab with the green dot\) use this JSON:

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
            "href": "https://<hostname>/harvesters/instances/snowplow-customer-se/state"
        }
    }
}
```

The deployment can take up to three minutes. You can check the current state like so:

![Harvester API: GET /harvesters/instances/snowplow-customer-se/state](../../../.gitbook/assets/image%20%286%29.png)

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "status": "RUNNING",
    "_links": {
        "self": {
            "href": "https://<hostname>/harvesters/instances/snowplow-customer-se/state"
        }
    }
}
```

## 4. Start Persister of Event Type "Customer Session"

Within step 2, we created a harvester. In order to be able to persist data in the event store, we need to start the persister of our event type "Customer Session". This goes like so:

For the body \(look for the tab with the green dot\) use this JSON:

![Harvester API: POST /event-types/customer-session/eventstores/pg/persister/state](../../../.gitbook/assets/image%20%2810%29.png)

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
            "href": "https://<hostname>/harvesters/instances/snowplow-customer-se/state"
        }
    }
}
```

The deployment can take up to two minutes. You can check the current state like so:

![Harvester API: GET /event-types/customer-session/eventstores/pg/persister/state](../../../.gitbook/assets/image%20%2825%29.png)

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "status": "DEPLOYING",
    "_links": {
        "self": {
            "href": "https://<hostname>/event-types/customer-session/eventstores/pg/persister/state"
        }
    }
}
```

