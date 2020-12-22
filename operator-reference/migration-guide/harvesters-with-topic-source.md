---
description: Migration guide to upgrade existing Harvesters with the Topic Source Type
---

# Harvesters with Topic Source

With the Release of Granary 1.0 the configuration of the [Topic Source Type](../../developer-reference/dataflow/data-in/source-types.md#topic-source) changed. ****

{% hint style="danger" %}
 Only Topic Source Harvesters with overwritten default configuration need to be migrated.
{% endhint %}

Configuration has been changed as follows:

| **Old Configuration** | New Configuration |
| :--- | :--- |
| consumer.concurrency | grnry-topic.concurrency |
| consumer.input-topic | grnry-topic.input-topic |
| consumer.resetOffsets | grnry-topic.reset-offsets |
| consumer.startOffset | grnry-topic.start-offset |

**Before 1.0 Aretha**

```text
{
    "name": "example-harvester",
    "displayName": "Example Harvester",
    "sourceType": {
        "name": "grnry-topic",
        "version": "0.9.4",
        "configuration": {
            "consumer.concurrency": 2,
            "consumer.input-topic": "test",
            "consumer.resetOffsets": "true",
            "consumer.startOffset": "earliest"
        }
        ...
    }
}
```

**With 1.0 Aretha**

```text
{
    "name": "example-harvester",
    "displayName": "Example Harvester",
    "sourceType": {
        "name": "grnry-topic",
        "version": "1.0.0",
        "configuration": {
            "grnry-topic.concurrency": 2,
            "grnry-topic.input-topic": "test",
            "grnry-topic.reset-offsets": "true",
            "grnry-topic.start-offset": "earliest"
        }
        ...
    }
}

```

