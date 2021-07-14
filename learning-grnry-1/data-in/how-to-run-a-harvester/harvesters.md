---
description: >-
  This page will give some background information about harvesters and on how to
  create and start them.
---

# Harvesters

## Prerequisites

Before you can create a harvester, you need to define an [event type](event-types.md) that categorizes the data _structure_ you want to import and a source type that provides a source app matching your data _source_ \(s3 / jdbc / webservice\).

## Harvester Definition

A harvester is the importing unit of Granary. It consists of

* a [_source_](../../../developer-reference/dataflow/data-in/source-types.md), that collects data from an external data source and makes it available to Granary, 
* a [_transform step_](../../../developer-reference/dataflow/data-in/scriptable-transform.md), that translates the incoming data into json format, and 
* a [_metadata extractor_ ](../../../developer-reference/dataflow/data-in/metadata-extractor.md)step, that creates the Granary headers to allow further processing. This step is configured within the event type.

A harvester is event-type-bound and will only process data provided by a single source app.

#### Harvester Output 

The steps above will emit events into a \(event type-specific\) kafka topic. So multiple harvesters can pull data of the same type \(like call records\) from different sources and write the data to the same kafka topic. But each harvester has also a dedicated "dead letter queue" kafka topic where unprocessable events \(=errors during transform, missing metadata\) will be written to. The dlq topic is created automatically on harvester creation. The name is part of the harvester entity \(`dlqTopic`\) and can not be changed.

## Creating a Harvester Instance \(also see [API Docs](../../../developer-reference/api-reference/harvester-api/harvester-instance-endpoints.md#create-harvester)\)

To create a new harvester you have to set at least the following fields:

| Attribute | Data Type | Description |
| :--- | :--- | :--- |
| `displayName` | string | This is what the the harvester instance will be called. This name needs to be unique. A unique technical name for the harvester instance will be derived from this field. See [hints](../best-practices-1/hints-on-naming-of-harvesters-and-event-types.md) on naming. |
| `sourceType` | object | You need to provide at least the `name:string` and the `version:string` of the desired source type. |
| `eventType` | object | You need to provide at least the `name:string` and the `version:string` of the event type, which you want the the harvester instance to process. |
| `description` | string | _Optional:_ You can provide a description for the the harvester instance. |
| `transform` | object | _Optional:_ Defines a scriptable transform application which the harvester instance should use. A default transform app will be deployed in case no custom definition is provided. Default values for all transform fields can be specified during the deployment of the harvester api.  |
| `metadataExtractor` | object | _Optional:_ Defines metadata extractor application that the harvester instance should use. A default metadata extractor app will be deployed in case no custom definition is provided. Default values for the metadataExtractor fields are specified during the deployment of the harvester api. |

{% hint style="warning" %}
Optional fields will be filled with pre-defined default values if not provided.
{% endhint %}

#### Example Create Call \(minimal configuration\)

```text
curl -X POST -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  -d @create-harvester.json \
  https://grnry-host/harvesters/instances
  
create-harvester.json content:

{
  "displayName": "harvester-1",
  "sourceType": {
    "name": "source-type-1",
    "version": "latest"
  },
  "eventType": {
    "name": "event-type-1",
    "version": "latest"
  }
}
```

This creates the following instance \(using default values for all non-provided configuration properties\)

```text
curl -X GET -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  https://grnry-host/harvesters/instances/harvester-1

{
  "name" : "harvester-minimal",
  "displayName": "harvester-minimal",
  "description": "myDescription",
  "dlqTopic": "harvester-dlq",
  "sourceType": {
    "name": "source-type-1",
    "version": "latest",
    "configuration": {},
    "deploymentConfiguration": {... default configuration options ...},
    "appConfiguration": {... default configuration options ...},
    "_links": {
      "type": {
        "href": "https://grnry-host/harvesters/source-types/source-type-1/latest"
      }
    }
  },
  "metadataExtractor": {
    "app": "default-app-meta",
    "version": "latest",
    "deploymentConfiguration": {... default configuration options ...},
    "appConfiguration": {... default configuration options ...}
  },
  "transform": {
    "app": "default-app-scriptable",
    "version": "latest",
    "deploymentConfiguration": {... default configuration options ...},
    "appConfiguration": {... default configuration options ...},
    "language": "groovy",
    "script": "return new String(payload, 'UTF-8');"
  },
  "eventType": {
    "name": "event-type-1",
    "version": "latest",
    "_links": {
      "self": {
        "href": "https://grnry-host/event-types/event-type-1/latest"
      }
    }
  },
  "links": {
    "self": {
      "href": "https://grnry-host/harvesters/instances/harvester-minimal"
    }
  }
}
```

#### Example Create Call \(full configuration\)

```text
curl -X POST -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  -d @create-harvester-full.json \
  https://grnry-host/harvester/instances

create-harvester-full.json contant:

{
  "displayName": "harvester-full",
  "description": "harvester with all configuration options explicitly set",
  "sourceType": {
    "name": "source-type-1",
    "version": "latest"
  },
  "eventType": {
    "name": "eventy-type-1",
    "version": "latest"
  },
  "metadataExtractor": {
    "app": "example-app-metadata",
    "version": "latest",
    "deploymentConfiguration": {
      "kubernetes.imagepullpolicy": "Always",
      "kubernetes.limits.cpu": "400m",
      "kubernetes.limits.memory": "256Mi",
      "kubernetes.requests.cpu": "400m",
      "kubernetes.requests.memory": "256Mi",
      "kubernetes.livenessprobedelay": "100",
      "kubernetes.readinessprobedelay": "100"
    },
    "appConfiguration": {
      "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
      "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
      "spring.cloud.stream.kafka.binder.autoaddpartitions": "false",
      "spring.cloud.stream.kafka.binder.consumer.maxattempts": "2",
      "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
      "spring.cloud.stream.kafka.binder.replicationfactor": "3"
    }
  },
  "transform": {
    "app": "example-app-scriptable",
    "version": "latest",
    "deploymentConfiguration": {
      "kubernetes.imagepullpolicy": "Always",
      "kubernetes.limits.cpu": "400m"
      "kubernetes.limits.memory": "256Mi",
      "kubernetes.requests.cpu": "400m",
      "kubernetes.requests.memory": "256Mi",
      "kubernetes.livenessprobedelay": "100",
      "kubernetes.readinessprobedelay": "100"
    },
    "appConfiguration": {
      "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
      "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
      "spring.cloud.stream.kafka.binder.autoaddpartitions": "false",
      "spring.cloud.stream.kafka.binder.consumer.maxattempts": "2",
      "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
      "spring.cloud.stream.kafka.binder.replicationfactor": "3"
    },
    "language", "groovy",
    "script", "return new String(payload, 'UTF-8');"
  }
}
```

This creates the following instance:

```text
curl -X GET -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  https://grnry-host/harvesters/instances/harvester-2

{
  "name" : "harvester-full",
  "displayName": "harvester-full",
  "description": "harvester with all configuration options explicitly set",
  "sourceType": {
    "name": "source-type-1",
    "version": "latest",
    "configuration": {},
    "deploymentConfiguration": {},
    "appConfiguration": {},
    "_links": {
      "type": {
        "href": "https://grnry-host/harvesters/source-types/source-type-1/latest"
      }
    }
  },
  "eventType": {
    "name": "event-type-1",
    "version": "latest",
    "_links": {
      "type": {
        "href": "https://grnry-host/event-types/event-type-1/latest"
      }
    }
  },
  "metadataExtractor": {
    "app": "example-app-metadata",
    "version": "latest",
    "deploymentConfiguration": {
      "kubernetes.imagepullpolicy": "Always",
      "kubernetes.limits.cpu": "400m",
      "kubernetes.limits.memory": "256Mi",
      "kubernetes.requests.cpu": "400m",
      "kubernetes.requests.memory": "256Mi",
      "kubernetes.livenessprobedelay": "100",
      "kubernetes.readinessprobedelay": "100"
    },
    "appConfiguration": {
      "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
      "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
      "spring.cloud.stream.kafka.binder.autoaddpartitions": "false",
      "spring.cloud.stream.kafka.binder.consumer.maxattempts": "2",
      "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
      "spring.cloud.stream.kafka.binder.replicationfactor": "3"
    }
  },
  "transform": {
    "app": "example-app-scriptable",
    "version": "latest",
    "deploymentConfiguration": {
      "kubernetes.imagepullpolicy": "Always",
      "kubernetes.limits.cpu": "400m"
      "kubernetes.limits.memory": "256Mi",
      "kubernetes.requests.cpu": "400m",
      "kubernetes.requests.memory": "256Mi",
      "kubernetes.livenessprobedelay": "100",
      "kubernetes.readinessprobedelay": "100"
    },
    "appConfiguration": {
      "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
      "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
      "spring.cloud.stream.kafka.binder.autoaddpartitions": "false",
      "spring.cloud.stream.kafka.binder.consumer.maxattempts": "2",
      "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
      "spring.cloud.stream.kafka.binder.replicationfactor": "3"
    },
    "language", "groovy",
    "script", "return new String(payload, 'UTF-8');"
  },
  "_links": {
    "self": {
      "href": "https://grnry-host/harvesters/instances/harvester-full"
    }
  }
}
```

## Updating a Harvester Instance \(also see [API Docs](../../../developer-reference/api-reference/harvester-api/harvester-instance-endpoints.md#update-harvester-instance)\)

You can update individual fields of a harvester configuration. If your configuration properties are identical to the current configuration the PUT call will return an empty `304 - NOT MODIFIED` response. If your call did update fields, it will return a `200 - SUCCESS`. If the harvester is already running it's state will change from `RUNNING` to `RUNNING_BUT_OUTDATED`.

Harvester name field name is not changeable and will be ignored if provided. Empty fields will be set to `""`, missing fields will remain unchanged. It is not possible to replace apps \(sourceType, metadataExtractor, transform\), only their versions and configs are modifiable.

#### Example Update Call

```text
curl -X PUT -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  -d @update-harvester.json \
  https://grnry-host/harvesters/instances/harvester-minimal
  
update-harvester.json content:

{
  "description": "new description"
}
```

This call updates the `description` of the previously created harvester.

## Starting And Stopping a Harvester \(also see [API Docs](../../../developer-reference/api-reference/harvester-api/harvester-instance-endpoints.md#start-stop-harvester-instance)\)

In order to see the current state of a harvester instance you can do:

```text
curl -X GET -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  https://grnry-host/harvesters/instances/harvester-minimal/state
  
RESPONSE:
{
  "status": "STOPPED",
  "_links": {
    "_self": {
      "href": "https://grnry-host/harvesters/instances/harvester-minimal/state"
    }
  }
}
```

To start/stop the harvester instance you need to send a POST Api Call to `/harvesters/instances/:harvester-name/state`.

#### Example Call

```text
curl -X POST -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  -d @harvester-state.json \ 
  https://grnry-host/harvesters/instances/harvester-minimal/state

harvester-state.json content:

{
  "action": "START"
}
```

{% hint style="info" %}
To stop a harvester instance the action field in `harvester-state.json` needs to be `"STOP"`
{% endhint %}

#### Different States of a Harvester Instance

A harvester instance can be in one of the following states:

| State | Description |
| :--- | :--- |
| **RUNNING** | The instance is currently running and operational. |
| **DEPLOYING** | The instance is currently deployed. |
| **RUNNING\_BUT\_OUTDATED** | The instance is currently running but there were configuration changes \(updates\) made to the definition of this harvester. The harvester will also be considered outdated, if the event type version in the harvester definition is `latest` and a new version of the event type has been published. The running harvester has to be restarted to apply these changes. |
| **FAILED** | The instance failed to deploy. |
| **STOPPING** | The instance is currently being stopped. |
| **STOPPED** | The instance has stopped. |
| **UNKNOWN** | The state of the instance is unknown. |

## Delete a Harvester Instance \(also see [API Docs](../../../developer-reference/api-reference/harvester-api/harvester-instance-endpoints.md#delete-a-harvester-instance)\)

#### Example DELETE call:

```text
curl -X DELETE -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  https://grnry-host/harvesters/instances/harvester-minimal
```

## Harvester Instance Logs \(also see [API Docs](../../../developer-reference/api-reference/harvester-api/harvester-instance-endpoints.md#get-harvester-instance-logs)\)

It is possible to access the logs of underlying apps of a harvester instance via the API.

#### Example Call

```text
curl -X GET -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  https://grnry-host/harvesters/instances/harvester-minimal/logs/sourceType&line=100
  
RESPONSE:
{
  "logs": [
    "\tgroup.id = g-h-snowplow-catch-all",
    "\theartbeat.interval.ms = 3000",
    "\tinterceptor.classes = []",
    "\tinternal.leave.group.on.close = true",
    "\tisolation.level = read_uncommitted",
    "\tkey.deserializer = class org.apache.kafka.common.serialization.ByteArrayDeserializer",
    "\tmax.partition.fetch.bytes = 1048576",
    "\tmax.poll.interval.ms = 300000",
    "\tmax.poll.records = 500",
    "\tmetadata.max.age.ms = 300000",
    "\tmetric.reporters = []",
    "\tmetrics.num.samples = 2",
    "\tmetrics.recording.level = INFO",
    "\tmetrics.sample.window.ms = 30000",
    "\tpartition.assignment.strategy = [class org.apache.kafka.clients.consumer.RangeAssignor]",
    "\treceive.buffer.bytes = 65536",
    "\treconnect.backoff.max.ms = 1000",
    "\treconnect.backoff.ms = 50",
    ...
  ]
}
```

You need to specify of which step you wish to get the logs from. There are only three options `sourceType`, `transform`, or `metadataExtractor`.

The path parameter `line` is optional and specifies how many lines the log should entail. Default is `500`.

## Full Example with cURL

#### Step 1: Create Event Type

```bash
 curl -X POST -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @event-type.json \
 https://grnry-host/event-types
```

{% code title="event-type.json" %}
```text
{
  "name" : "test-event-type-post",
  "correlationIdExpression" : "#jsonPath(#jsonPath(payload, 'body'), 'data[0].correlationId')",
  "eventIdExpression" : "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].eventId')?:#randomUUID()",
  "timestampExpression" : "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].timestamp')?:#nowMillis()"
}
```
{% endcode %}

#### Step 2: Start Persister Stream

```bash
curl -X POST -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @persister-start.json \    # start body
 https://grnry-host/event-types/test-event-type-post/eventstores/pg/persister/state
```

{% code title="persister-start.json" %}
```text
{
  "action" : "START"
}
```
{% endcode %}

#### Step 3: Create Source Type \(App already registered with SCDF\):

If Source Type not already present in the database:

```text
INSERT INTO source_types (
    name, 
    version, 
    description, 
    uri, 
    metadata_uri, 
    deployer_config, 
    app_config
)
VALUES (
    'jdbc',
    'latest',
    'jdbc source implementation - grnry',
    'docker://host.com/service/service-jdbc-source:latest',
    'maven://org.springframework.cloud.stream.app:jdbc-source-kafka:jar:metadata:2.1.2.RELEASE',
    '{"kubernetes.imagePullPolicy": "Always", "kubernetes.limits.cpu": "400m"}',
    '{"spring.cloud.stream.bindings.output.producer.partitionCount": 12, "spring.cloud.stream.bindings.output.producer.autoAddPartitions": true}'
)
ON conflict DO NOTHING
```

Register app in spring cloud data flow:

```bash
curl -X POST -u <user>:<password> -d \
"uri=docker://host.com/service/service-jdbc-source:latest&metadata-uri=maven://org.springframework.cloud.stream.app:jdbc-source-kafka:jar:metadata:2.1.2.RELEASE" \
https://scdf-host/apps/source/jdbc/latest
```

#### Step 4: Create Harvester Instance

```text
curl -X POST -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  -d @create-harvester.json \
  https://grnry-host/harvesters/instances
```

{% code title="create-harvester.json" %}
```text
{
  "displayName": "harvester-1",
  "sourceType": {
    "name": "jdbc",
    "version": "latest"
  },
  "eventType": {
    "name": "test-event-type",
    "version": "latest"
  }
}
```
{% endcode %}

#### Step 5: Start Harvester Stream

```text
curl -X POST -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  -d @harvester-start.json \
  https://grnry-host/harvesters/instances/harvester-name/state
```

{% code title="harvester-start.json" %}
```text
{
  "action" : "START"
}
```
{% endcode %}

#### Step 6: Cleanup \(optional\)

If you want to remove the previously created definitions \(and deployed stream\) you need to delete the harvester first:

```text
curl -X DELETE -H "Content-Type: application/json" \
  -H "Authorization: Bearer <Access-Token>" \
  https://grnry-host/harvesters/instances/harvester-1
```

and afterwards the event type:

```bash
curl -X DELETE -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 https://grnry-host/event-types/test
```

