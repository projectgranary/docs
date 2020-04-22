---
description: >-
  A Springboot/Spring batch microservice that fetches time-to-live (TTL) expired
  Grains from the Profilestore and send them to a Kafka topic.
---

# Reaper

![Reaper Data Flow from Profile Store to Belt Extractor ](../../../.gitbook/assets/reaper%20%281%29.PNG)

The Granary Reaper is responsible for collecting expired grains by querying them from the [Profile Store](./) according to a CRON schedule and writing them back into the data pipeline through a TTL Kafka topic. 

## Configuration

| Parameter Name | Description | Default value |
| :--- | :--- | :--- |
| IO\_GRNRY\_REAPER\_BACKWARDS\_COMPATIBLE | Reaper runs on Granary 0.7 feature-level | true |
| IO\_GRNRY\_REAPER\_GRAIN\_FETCH\_SIZE | Job batch size of grains to read | 100 |
| IO\_GRNRY\_REAPER\_OUTPUT\_TOPIC\_PREFIX | User-defined prefix of the output topic  | grnry\_data\_in\_ |
| IO\_GRNRY\_REAPER\_DATASOURCE\_TABLE\_NAME | Table name of Profile Store | public.profilestore |
| PROFILESTORE\_HOSTNAME | Hostname of Profile Store | grnry-pg-citus |
| PROFILESTORE\_PORT | Port of Profile Store | 5432 |
| PROFILESTORE\_DB\_NAME | Database name of Profile Store | postgres |

#### Profile Type Naming Convention

Reaper's TTL Kafka topics naming schema: `grnry_data_in_<profile-type>`. 

Profile type here refers to the type of profile as seen in the [Profile Store](./#table-profilestore), with underscores `_`normalized into hyphens `-`. If the resulting topic name exceeds Kafka's limit of 249 characters, it is cut off at the maximum possible length. 

{% hint style="warning" %}
Please note that the Reaper's TTL Kafka topic\(s\) will have to be manually created if Kafka's `auto.create.topics.enable` is set to `false`. When manually creating the topics, please align the retention with the cron schedule of the Granary Reaper.
{% endhint %}

## Output

Please note that to comply with the data protection law, TTL grains will not carry a value. 

### Kafka Message Header Fields

Description of the header fields can be found in the [Belt Extractor](../belt-extractor.md#callback-signature) specification.

| Field Name | Value |
| :--- | :--- |
| topic | `{outputTopicPrefix} + {normalized profile type (replacing all '_' with '-')}` |
| kafka\_messageKey | `{profile type}` |
| grnry-harvester-name | `"grnry-reaper"` |
| grnry-correlation-id | `{correlation id}` |
| grnry-event-timestamp | `{reaper processing time timestamp}` |
| grnry-event-id | `{reaper generated event UUID}` |
| grnry-event-type | `{normalized profile type}` |

### Kafka Message Payload Fields

| Field name | Description |
| :--- | :--- |
| correlationId | The grains correlation ID |
| profileType | The grains profile type |
| path | The grains path |
| pit | The grains pit |
| grain\_type | The grains type |
| inserted | The grains profile store insertion timestamp  |
| ttl | The grains ttl \(ISO8601\) |
| reader | The grains reader |
| origin | The grains origin |

