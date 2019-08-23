---
description: >-
  On this page, available Granary SCDF components and their corresponding
  parameters are described.
---

# Shared parameters

In this chapter, we are going to have a look at the details and different parameters of the components shipped with GRNRY.

The following parameters are shared across all Granary SCDF extensions:

```text
grnry.harvesterName
```

In this parameter you define the name of the harvester. This functions as a grouping parameter to deploy the whole pipeline as one and enable common metrics.

```text
grnry.eventTypeName
```

This parameter stores the name of the event, the harvester is processing. Remember, each harvester processes exactly one event.

{% hint style="info" %}
Due to technical limitations in the [Event Store API](../../api-reference/event-store-api.md), the `eventTypeName` may not contain "`_`".
{% endhint %}

## Dead letter queues

In GRNRY, we have created so called _dead letter queues_. These dead letter queues are used to receive all the data that could not be processed correctly by the transform or metadata extractor steps. They are the output channels for errors. Dead letter queues are available for the [transform](scriptable-transform.md) and the [metadata extractor](metadata-extractor.md) steps.

### Mandatory parameters

In order to create these dead letter queues you have to define at least one parameter:

```yaml
"spring.cloud.stream.bindings.harvesterDlq.destination": <dlq_name>
```

In this sample &lt;dlq\_name&gt; refers to the name of the Kafka Topic the erroneous data should be sent to. As a convention you should use: 

```yaml
"grnry_harvester_${grnry.harvesterName}_dead_letter"
```

`${grnry.harvesterName}` is the variable you have entered for the harvester. Whenever this parameter is not set in your deployment, it will automatically default to: `harvesterDlq`. As this is not very expressive, you should definitely enter a DLQ as proposed above.

### Optional parameters

In addition, there are two further parameters:

```yaml
"spring.cloud.stream.bindings.harvesterDlq.producer.partitionCount": 6
"spring.cloud.stream.bindings.harvesterDlq.producer.autoAddPartitions": true
```

The first parameter defines how many partitions there should be at least for the dead letter queue \(more are possible\) and the second one defines whether it is okay to automatically add partitions.

### When is something written to the Dead Letter Queue?

The result is, that whenever there is an error in your transform or metadata extractor step, the data is sent to the dead letter queue. There you get the description of the error and the **original** payload. The original payload refers to the data you have received as input for the transform step, meaning the input to your harvester after the source type. By doing so, we make sure, you do not drop the original data of your request and can reprocess it, if wanted.



