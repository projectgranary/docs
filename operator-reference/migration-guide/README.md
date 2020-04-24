---
description: Explanation of migration strategies between Granary platform versions.
---

# Migration guide

## Upgrade from Granary 0.7 "Kurt" to Granary 0.8 "Lemmy"

### Alter table "public.eventstore"

```text
ALTER TABLE public.eventstore ADD COLUMN IF NOT EXISTS event_type_version bigint NOT NULL DEFAULT 1;
```

### Install new Granary Components

* [Harvester API](../installation/harvester-api.md)
  * [Make sure to have registered SCDF Applications in SCDF Server before starting Harvester API](../../learning-grnry-1/data-in/how-to-run-a-harvester/getting-started.md)
  * [Source Types need to be added manually via SQL](../../learning-grnry-1/data-in/how-to-run-a-harvester/source-types.md#registering-a-new-source-type)
* [Segment Manager](../installation/segment-manager.md)
* [Segment Management API](../installation/segment-creation-api.md)
* [Reaper](../installation/reaper.md)

### Update Granary Components

Update all remaining Granary components to their Granary 0.8 version as denoted in the [release notes](../granary-release-notes/). Bolt version indicate an update.

### Update Granary Harvesters to use Harvester API

Harvester Data-in pipeline definition changed with Granary 0.8. See the [Harvester API CI/CD Cookbook](ci-cd-cookbook.md) for details.

### Update Granary Segments to use Segment Managment API

Segment Creation flow is now Rest API based and does no longer require Helm deployments. See [Segment Job Migration](segment-job-migration.md) for instructions.

### Update Granary Reaper Data Flows

Granary's Reaper got introduced in Granary 0.7. In this version, Kafka topics for TTL-expired grains had to be created manually due to required custom retentions. This is no longer needed in Granary 0.8 because Granary's Reaper creates the Kafka topics for TTL-expired grains automatically via Harvester API's Event Type endpoints. However, Belts consuming TTL-expired grains need to be updated in order to consume those TTL Event Types. 

**That's it.**

