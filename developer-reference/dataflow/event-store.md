---
description: >-
  https://gitlab.alvary.io/grnry/kafka-connect/tree/master/kafka-connect-to-postgres
---

# Event Store

![](../../.gitbook/assets/eventstore.png)

## Table `'eventstore'`

{% tabs %}
{% tab title="Spec" %}
| Key | Description | Data Type | Default | Null |
| :--- | :--- | :--- | :--- | :--- |
| **correlation\_id**  | Correlation-ID. Groups all events that belong to the same tracking entity \(cookie, device, customer, contact, claim, etc.\). | varchar | - | `NOT NULL` |
| **event\_id**  | Event-ID. Used to deduplicate events. | uuid | - | `NOT NULL` |
| created  | Created. The timestamp of the original event creation. | bigint | - | `NOT NULL` |
| message  | Message. The original payload of the event as it was ingested into Granary encoded as JSON. | jsonb | - | - |
| event\_type | Event type. Describes type of the event. The type is customizable in Metadata Extractor. This is used to control access to the event. | varchar | `na` | `NOT NULL` |
| event\_harvester | Event harvester. Describes the source of the event. The source is the Granary Harvester of the event. This is used to control access to the event. | varchar | `na` | `NOT NULL` |
{% endtab %}

{% tab title="Example" %}
| correlation\_id | event\_id | created | message | event\_type | event\_harvester |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 8714343 | bfa9c1f5-7ae0-4c92-a067-d4ef9fe3927f | 1542020155031 | { "body": "{ ... }", "userAgent": "Mozilla/5.0 \(Windows NT 10.0; Win64; x64\) AppleWebKit/537.36 \(KHTML, like Gecko\) Chrome/67.0.3396.99 Safari/537.36" } | web | snowplow |
{% endtab %}
{% endtabs %}



