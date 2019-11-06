---
description: Explanation of data migration strategies between Granary platform versions.
---

# Migration guide

## Upgrade from Granary 0.6 "Freddie" to Granary 0.7 "Kurt"

To allow a seamless upgrade, only one SQL update on "public.belts" table  are needed for Belt API.

### Update "public.belts" 

```text
UPDATE public.belts SET extractor_version = 'latest' where extractor_version ='';
```

### Update Granary Components

Update all remaining Granary components to their Granary 0.7 version as denoted in the [release notes](granary-release-notes/). Bolt version indicate an update.

**That's it.**

