---
description: Denotes release versions of Granary artifacts.
---

# Granary Release Notes

## Release Version Schema

### Granary Platform

* [Semantic Release](https://semver.org/) version on `major.minor` level for Granary platform releases and its corresponding features and documentation.
  * e.g. `1.3` 
* Granary `major.minor.patch` release version defines exactly one combination of Granary components' `patch` level releases.

### Granary Components

* Semantic Release version on `major.minor.patch` level for Docker Container and Helm Chart.
  * e.g. `1.3.2` 
* Component release versions can differ from Granary Platform on `major`, `minor`, and `patch` level.

## 0.4.2 "Jimi" - 2019-06-14

| Granary Component | Release Version | Release Notes |
| :--- | :--- | :--- |
| [Ambassador](https://github.com/helm/charts/tree/master/stable/ambassador) | 0.40.1 |  |
| [Snowplow Scala Stream Collector API](../installation/snowplow-scala-stream-collector.md) | 0.4.2 \(based on Snowplow v0.15.0\) |  |
| [Metadata Extractor](../installation/metadata-extractor.md) | 0.4.3 |  |
| [Event Store API](../installation/event-store-api.md) | 0.4.3 |  |
| [Granary Kafka Connect](../installation/granary-kafka-connect.md) | 0.4.1 \(based on Confluent Kafka Connect 5.1.2\) |  |
| [Belt Extractor](../installation/untitled.md) | **0.4.8** | Introducing new Array Operations to Belt Extractor Runtime as defined in [https://gitlab.alvary.io/grnry/scrum/issues/228](https://gitlab.alvary.io/grnry/scrum/issues/228). |
| [Profile Updater](../installation/profile-updater.md) | **0.4.3** | Introducing new Array Operations to Profile Udpater as defined in [https://gitlab.alvary.io/grnry/scrum/issues/228](https://gitlab.alvary.io/grnry/scrum/issues/228). |
| [Profile Store API](../installation/profile-store-api.md) | 0.4.2 |  |
| [Segment Table Creator](../../developer-reference/dataflow/segment-store.md) | 0.4.2 |  |
| [Segment Store API](../installation/segment-store-api.md) | 0.4.3 \(based on Presto 0.216\) |  |
| \*\*\*\*[Granary UI](../installation/granary-ui.md) | 0.1.0 |  |
| [GraphQL API](../installation/graphql-api.md) | 0.1.0 \(based on Apollo Server 2.4.8\) |  |
| \*\*\*\*[Citus PostgreSQL](../installation/citus-postgresql.md) | 8.1.1 |  |
| [Confluent Kafka](https://github.com/confluentinc/cp-helm-charts/tree/master/charts) | 5.1.2 |  |
| [Confluent Zookeeper](https://github.com/confluentinc/cp-helm-charts/tree/master/charts) | 5.1.2 |  |
| [Kafka Manager](../installation/kafka-manager.md) | 0.4.0 |  |
| [Keycloak](https://github.com/helm/charts/tree/master/stable/keycloak) | 4.5.0.Final |  |
| [Grafana](https://github.com/helm/charts/tree/master/stable/grafana) | 5.2.3 |  |

## 0.4.1 "Jimi" - 2019-05-14

| Granary Component | Release Version |
| :--- | :--- |
| [Ambassador](https://github.com/helm/charts/tree/master/stable/ambassador) | 0.40.1 |
| [Snowplow Scala Stream Collector API](../installation/snowplow-scala-stream-collector.md) | 0.4.2 \(based on Snowplow v0.15.0\) |
| [Metadata Extractor](../installation/metadata-extractor.md) | 0.4.3 |
| [Event Store API](../installation/event-store-api.md) | 0.4.3 |
| [Granary Kafka Connect](../installation/granary-kafka-connect.md) | 0.4.1 \(based on Confluent Kafka Connect 5.1.2\) |
| [Belt Extractor](../installation/untitled.md) | **0.4.7** |
| [Profile Updater](../installation/profile-updater.md) | **0.4.2** |
| [Profile Store API](../installation/profile-store-api.md) | 0.4.2 |
| [Segment Table Creator](../../developer-reference/dataflow/segment-store.md) | 0.4.2 |
| [Segment Store API](../installation/segment-store-api.md) | 0.4.3 \(based on Presto 0.216\) |
| \*\*\*\*[**Granary UI**](../installation/granary-ui.md)\*\*\*\* | **0.1.0** |
| \*\*\*\*[**GraphQL API**](../installation/graphql-api.md)\*\*\*\* | **0.1.0 \(based on Apollo Server 2.4.8\)** |
| \*\*\*\*[Citus PostgreSQL](../installation/citus-postgresql.md) | 8.1.1 |
| [Confluent Kafka](https://github.com/confluentinc/cp-helm-charts/tree/master/charts) | 5.1.2 |
| [Confluent Zookeeper](https://github.com/confluentinc/cp-helm-charts/tree/master/charts) | 5.1.2 |
| [Kafka Manager](../installation/kafka-manager.md) | 0.4.0 |
| [Keycloak](https://github.com/helm/charts/tree/master/stable/keycloak) | 4.5.0.Final |
| [Grafana](https://github.com/helm/charts/tree/master/stable/grafana) | 5.2.3 |

## 0.4.0 "Jimi" - 2019-04-05

| Granary Component | Release Version |
| :--- | :--- |
| [Ambassador](https://github.com/helm/charts/tree/master/stable/ambassador) | 0.40.1 |
| [Snowplow Scala Stream Collector API](../installation/snowplow-scala-stream-collector.md) | 0.4.2 \(based on Snowplow v0.15.0\) |
| [Metadata Extractor](../installation/metadata-extractor.md) | 0.4.3 |
| [Event Store API](../installation/event-store-api.md) | 0.4.3 |
| [Granary Kafka Connect](../installation/granary-kafka-connect.md) | 0.4.1 \(based on Confluent Kafka Connect 5.1.2\) |
| [Belt Extractor](../installation/untitled.md) | 0.4.6 |
| [Profile Updater](../installation/profile-updater.md) | 0.4.1 |
| [Profile Store API](../installation/profile-store-api.md) | 0.4.2 |
| [Segment Table Creator](../../developer-reference/dataflow/segment-store.md) | 0.4.2 |
| [Segment Store API](../installation/segment-store-api.md) | 0.4.3 \(based on Presto 0.216\) |
| [Citus PostgreSQL](../installation/citus-postgresql.md) | 8.1.1 |
| [Confluent Kafka](https://github.com/confluentinc/cp-helm-charts/tree/master/charts) | 5.1.2 |
| [Confluent Zookeeper](https://github.com/confluentinc/cp-helm-charts/tree/master/charts) | 5.1.2 |
| [Kafka Manager](../installation/kafka-manager.md) | 0.4.0 |
| [Keycloak](https://github.com/helm/charts/tree/master/stable/keycloak) | 4.5.0.Final |
| [Grafana](https://github.com/helm/charts/tree/master/stable/grafana) | 5.2.3 |

