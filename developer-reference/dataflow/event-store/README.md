---
description: This page outlines the storage of raw event data
---

# Event Store

![Data flow within raw data zone of Granary](../../../.gitbook/assets/events.PNG)

In the raw data zone of Granary, each message of an [Event Type](../../../learning-grnry-1/data-in/how-to-run-a-harvester/event-types.md) is persisted in the Event Store table by the [SCDF Event Store sinks](../data-in/eventstore-sink.md). Also, raw data can be replayed back into belts by the Event Feeder.

Data can be retrieved from Event Store using the [Event Store API](../../api-reference/event-store-api.md).

## Table `eventstore`

{% tabs %}
{% tab title="Spec" %}
| Key                 | Description                                                                                                                                                                                           | Data Type | Default | Null       |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ------- | ---------- |
| **correlation_id ** | Correlation-ID. Groups all events that belong to the same tracking entity (cookie, device, customer, contact, claim, etc.).                                                                           | varchar   | -       | `NOT NULL` |
| **event_id **       | Event-ID. Used to deduplicate events.                                                                                                                                                                 | uuid      | -       | `NOT NULL` |
| created             | Created. The timestamp of the original event creation.                                                                                                                                                | bigint    | -       | `NOT NULL` |
| message             | Message. The original payload of the event as it was ingested into Granary encoded as JSON.                                                                                                           | jsonb     | -       | -          |
| event_type          | Event type. Describes type of the event. The type is customizable in Metadata Extractor. This is used to control access to the event. `event_type` must not include "`_`".                            | varchar   | `na`    | `NOT NULL` |
| event_harvester     | Event harvester. Describes the source of the event. The source is the Granary Harvester of the event. This is used to control access to the event. `event_harvester` must not include "`_`".          | varchar   | `na`    | `NOT NULL` |
| partition_id        | kafka partition the message was read from                                                                                                                                                             | bigint    | -       | `NOT NULL` |
| partition_offset    | partition offset the message was read from                                                                                                                                                            | bigint    | -       | `NOT NULL` |
| ttl                 | Interval representation that determines the time between creation and deletion of an event. Must be set explicitly via deletion-event to mark an event for reaper deletion. Always null on insertion. | varchar   | NULL    | -          |
{% endtab %}

{% tab title="Example" %}
| correlation_id | event_id                             | created       | message                                                                                                                                                  | event_type | event_harvester | partition_id | partition_offset | ttl  |
| -------------- | ------------------------------------ | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------ | ---------------- | ---- |
| 8714343        | bfa9c1f5-7ae0-4c92-a067-d4ef9fe3927f | 1542020155031 | { "body": "{ ... }", "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36" } | web        | snowplow        | 20           | 225102           | NULL |
{% endtab %}
{% endtabs %}

##

