---
description: This page describes how you can access GRNRY components in Kibana.
---

# Discovering Logs in Kibana

Granary recommends to use a logging backend as detailed on the [Common Log Format](../../operator-reference/site-reliability/common-log-format.md#recommended-logging-architecture) page. In case your Granary instance features a logging backend with a Kibana as log frontend, you can discover platform and data pipeline logs with the following `kubernetes.labels`.

### Discovering Platform Components

To discover components in Kibana you can select platform components by their field `kubernetes.labels.app`. To select, for instance, the Harvester API you need to query `kubernetes.labels.app=harvester-api`. 

### Discovering Data Pipeline Components

Data Pipeline components can also be discovered in Kibana.

#### Harvesters

To select harvester, you need to use the field `kubernetes.labels.spring-group-id`. For example, query like this: `kubernetes.labels.spring-group-id=g-h-snowplow-a-std`. 

The `spring-group-id` label is equivalent to the harvester's `streamName` in the Granary UI.

#### Belts

To select belts, you need to use the field `kubernetes.labels.beltName`. For example, query like this: `kubernetes.labels.beltName=grnry-belt-1`. 

The `beltName` label is equivalent to the belt's `kubernetesName` in the Granary UI.

#### 

