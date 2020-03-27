---
description: >-
  This page introduces Granary specific terminology to the reader. Some of the
  terms complement the agricultural notion of "Granary".
---

# Granary Glossary

| Term | Description |
| :--- | :--- |
| \(Data Out\) Channels | Application that consums data from Granary \(i.e. Adobe, Mailchimp, …\). |
| Belt | Function-as-a-service like interface to enrich raw events with business logic to form profiles. It is fully programmable by users. |
| Belt UI & API | UI and API to manage & deploy belts in Kubernetes. |
| Belt Extractor Runtime | The currently running instance of a belt which is managed by the Belt API. |
| Correlation ID | Unique ID of a \(profile\) entity such as customer, contract, or mail which is used to identify an entity within the Profile Store. |
| Data Science Work Bench | A component for AI modeling purposes accessing the Event Store. Based on Jupyter Hub, a data science GUI. |
| Event Feeder | Arranges raw events according to a 'history' order to enable data replay. In addition, it ensures that via a first belt 'older' events are processed before a second belt starts processing 'current' events. |
| Event Tracing | Attaches metadata to each event for tracing data flow within Granary. |
| Event Type | Extracted type of a raw event based on metadata and schema. Granary uses event types to control data flow. |
| Event Store | A persistent store of raw events which have already been processed by Harvesters. The events have been translated into a common format and in addition metadata has been added to the original event. |
| Explorer UI | UI showing Grains and relations of Profiles based on Correlation IDs. |
| GDPR Reaper | GDPR data check for data points. Given an invalid 'time to live', the reaper will be deleting or anonymizing data points. |
| Grain | A single/array value which is written by belts and contains information about a profile entity. |
| Harvester | Combination of a data source, a JSON transform step and a metadata \(Event ID and Correlation ID\) extraction to "harvest" data from an outside system. |
| Harvester Framework | An API-drive data ingestion framework to connect real-time and batch data sources to Granary. |
| Harvester UI & API | UI and API to configure and deploy Harvesters. |
| Kafka | Realtime Event Log managing the communication between all components of data-in and data-out processes within Granary. |
| Keycloak | System for role and idenity management ensuring correct data visibility. Currently supporting Segment Store, Profile Store, Event Store, Harvester API and Belt API. |
| Kubernetes | An environment used for running containerized Cloud Native applications and a hardware ressource scheduler. |
| Profiles API, Event API | APIs for extracting Events and Profiles as JSON files given the Keycloak access rights. Not supporting SQL. |
| Profile Store | Key-value like store to persist profile entities containing \(linked\) grains. |
| Segments | Structured view on Profile Store or Event Store given an analytical requirement/need. |
| Segment Manager | Kubernetes Controller to reconcile segment custom resources to segment cron jobs. |
| Segment Management API | API to CRUD segment custom resources against the Kubernetes API. |
| Segment Store | SQL interface to query segments. |
| Sessionizing | Specific session collecting events and processing en bloc \(e.g. within a session campaign success\) |
| Tracking API | Snowplow Collector API to collect web-tracking events from webpages, mobile apps, etc. |

