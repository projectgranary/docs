---
description: Explanation of data migration strategies between Granary platform versions.
---

# Migration guide

## Upgrade from Granary 0.4 "Jimmy" to Granary 0.5 "Amy"

To allow a seamless upgrade, version 0.5 components will be deployed in parallel with already running version 0.4 components. Already existing components and related topics will not be changed during upgrade.

### Reconfigure Keycloak

* New clients for e.g. Spring Cloud Data Flow \(SCDF\) and Belt API 
* New roles for belts
* Create technical users \(e.g. belts trying to access Profile Store API\)

### Migrate citus database

* Create new Event Store table
* Create SCDF schemas \(scdf and skipper\)
* Create Belt API tables

### Install SCDF

[Install SCDF using helm chart.](installation/spring-cloud-data-flow.md)

### Install Belt API

[Install Belt API using helm chart.](installation/belt-api.md)

### Create harvester configuration derived from 0.4 Metadata extraction rules

A dedicated harvester for each `Event Type` needs to be created. Therefore, take a look at the current configuration of the grnry-extractor and as the `Event Types` might be dynamically created from the payload itself. This entails writing a script that transform the incoming data into a json format in the scriptable transform and to configure the metadata extractor according to the old rule configuration of [Granary Extractor](../developer-reference/dataflow/data-in/metadata-extractor.md). See [Get data into Granary](../learning-grnry-1/data-in/) section to get help with the new configuration. This step should result in a **Harvester Configuration** and **Eventstore-Sink Configuration**. 

{% hint style="info" %}
Query the current event-store table to find out which event types are getting actually created.
{% endhint %}

### Update non-breaking GRNRY components

Upgrade \(using rolling upgrade\) non-breaking components to [their 0.5 version](granary-release-notes/) using their related helm chart. There is no configuration change of the value files required.

* [Profile Updater](installation/profile-updater.md)
* [Profile Store API](installation/profile-store-api.md)
* [Segment Store API](installation/segment-store-api.md)

Since the Profile Updater service is independent from the Belt Extractor runtime version, it can be upgraded beforehand although it is used by belts.

### Create Kafka topics

Create following Kafka topics manually using Kafka Manager or Kafka CLI.

Introduce a new topic \(hereinafter referred to as topic named\) '`snowplow`' and deploy a parallel setup of version 0.5 compatible harvesters to consume data from topic "snowplow". One harvester is required for every `Event Type` introduced.

* create '`snowplow`' topic
* create event type specific topics \( '`grnry_data_in_[event-type]`'.\)

### Migrate Belts

Since version 0.5.0 of [Belt Extractor](../developer-reference/dataflow/belt-extractor.md) runtime the signature of the execute function of the belt includes a third parameter \(`grnry-headers`\), which replace the information that formerly was available in the events in `paylod_id`. Also the `profile-type` can be set now in the update messages and should be considered when migrating.  

### Deploy EventStore Sinks

[Deploy using SCDF API.](../learning-grnry-1/data-in/getting-started.md#creating-an-eventstore-sink)

{% hint style="info" %}
Ensure to use a explicit groupId so that groupId can be used again when upgrading from 0.5 to future 0.6 \(data-in api\)
{% endhint %}

### Deploy harvester definitions

[Deploy using SCDF API.](../learning-grnry-1/data-in/getting-started.md#creating-a-data-pipeline)

{% hint style="info" %}
Ensure to use explicit groupIds so that groupIds can be used again when upgrading from 0.5 -&gt; 0.6 \(data-in api\)
{% endhint %}

### Deploy migrated belts via Belt Extractor runtime

Each current belt needs to be refactored to match the new callback structure. Create and deploy these refactored belts via the [Belt API](../developer-reference/api-reference/belt-api.md). \(Currently running belts will not be upgraded\).

### Configure Snowplow Collector

Reconfigure the current [Snowplow helm-based deployment](installation/snowplow-scala-stream-collector.md) to write to "snowplow" instead of "raw" and do a rolling upgrade using helm upgrade. This will route new incoming events through the version 0.5 pipeline. Besides this, there are still pending events being processed via the 0.4 pipeline.

{% hint style="info" %}
Topic configuration is stored in a configmap, so you might need to kill the pods one by one if the rolling upgrade does not restart the pods.
{% endhint %}

### Wait and data QA

Check for data income in topic "snowplow" using Kafka. After doing further data checks in realtime data, shutdown grnry-extractor and version 0.4 belts.

### Remove old components

Remove old helm deployments of Kafka Connect as well as Metadata Extractor and the migrated version 0.4 belts using `helm delete`.

### Migrate old event data to new eventstore

Migrate old Event Store using the helm chart that can be found in the [https://gitlab.alvary.io/grnry/eventstore\_migration\_04\_05](https://gitlab.alvary.io/grnry/eventstore_migration_04_05) repository.

### Upgrade Eventstore API

Upgrade eventstore-api using helm chart `grnry-stable/eventstore-api`.  
Ensure to fit the value file matching the new table name. By default, this is `public.eventstore`.

That's it.

