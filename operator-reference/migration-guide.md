---
description: Explanation of data migration strategies between Granary platform versions.
---

# Migration guide

## Upgrade from Granary 0.5 "Amy" to Granary 0.6 "Freddie"

To allow a seamless upgrade, only three SQL statements are needed for Belt API.

### Alter table "public.belts"

```text
ALTER TABLE public.belts ADD COLUMN IF NOT EXISTS kafka_destination_topic VARCHAR (255) NOT NULL DEFAULT 'profile-update';
```

### Update table "public.belts" 

```text
UPDATE public.belts SET extractor_version = 'latest' where extractor_version ='';
```

### Alter table "public.belts\_log"

```text
ALTER TABLE public.belts_log ADD COLUMN IF NOT EXISTS kafka_destination_topic VARCHAR (255) NOT NULL DEFAULT 'profile-update';
```

### Install Zipkin Server

Zipkin Server is the only new component in Granary 0.6. Install it as described in [Install Zipkin](installation/zipkin.md).

### Update Granary Components

Update all remaining Granary components to their Granary 0.6 version as denoted in the [release notes](granary-release-notes/). Bolt version indicate an update.

**That's it.**

