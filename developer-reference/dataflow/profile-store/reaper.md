---
description: >-
  A Springboot/Spring batch microservice that fetches ttl-outdated Profiles from
  the Profilestore and send them to a Kafka topic.
---

# Reaper

The Granary Reaper is responsible for collecting expired grains, by querying them from the [Profile Store](./) and writing them back into the data pipeline through a TTL Kafka topic. 

## Configuration

| Parameter Name | Description | Default value |
| :--- | :--- | :--- |
| IO\_GRNRY\_REAPER\_BACKWARDS\_COMPATIBLE | Flag indicating if Reaper runs on 0.7 feature-level, for 0.8+ should be set to `false` | true |
| IO\_GRNRY\_REAPER\_HARVESTER\_API\_URL | Harvester API URL | https://localhost:8080 |
| IO\_GRNRY\_REAPER\_AUTHENTICATION\_URL | Keycloak URL | https://localhost:8080 |
| IO\_GRNRY\_REAPER\_CONNECTION\_RETRIES | Number of retries to make on failing HTTP requests | 5 |
| IO\_GRNRY\_REAPER\_KEYCLOAK\_REALM | Keycloak Realm | grnry |
| IO\_GRNRY\_REAPER\_KEYCLOAK\_CLIENT\_ID | Keycloak Client ID | harvester-api |
| IO\_GRNRY\_REAPER\_GRAIN\_FETCH\_SIZE | Job batch size of grains to read | 100 |
| IO\_GRNRY\_REAPER\_EVENT\_TYPE\_RETENTION\_MS | Kafka retention for TTL Event Types created by the Reaper | 300000 |
| IO\_GRNRY\_REAPER\_EVENT\_TYPE\_PARTITIONCOUNT | Kafka partition count for TTL Event Types created by the Reaper | 24 |
| IO\_GRNRY\_REAPER\_EVENT\_TYPE\_REPLICATION | Kafka replication for TTL Event Types created by the Reaper | 2 |
| IO\_GRNRY\_REAPER\_EVENT\_TYPE\_TTL | ISO8601 TTL for TTL Event Types created by the Reaper | P100Y |
| IO\_GRNRY\_REAPER\_DATASOURCE\_TABLE\_NAME | Table name of Profile Store | profilestore |
| PROFILESTORE\_HOSTNAME | Hostname of Profile Store | grnry-pg-citus |
| PROFILESTORE\_PORT | Port of Profile Store | 5432 |
| PROFILESTORE\_DB\_NAME | Database name of Profile Store | postgres |

## Output

Please note that to comply with the data protection law, TTL grains will not carry a value. 

### Kafka Message Header Fields

Description of the header fields can be found in the [Belt Extractor](../belt-extractor/#callback-signature) specification.

| Field Name | Value |
| :--- | :--- |
| topic | `{fetched from Harvester API event type endpoints}` |
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
