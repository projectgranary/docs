---
description: Migration guide to upgrade to Reaper 1.1
---

# Reaper 1.1

Granary 1.1 introduces a new column that needs to be indexed for improved reaper performance. If the Profile Updater is already on Granary 1.1, this will be taken care of by flyway. Otherwise you should add the index with the following statement:

```sql
CREATE INDEX IF NOT EXISTS profilestore_time_to_notify ON public.profilestore (profile_time_to_act(inserted, ttn));
```

For using the notification and deletion functionality in Granary 1.1 you now need to deploy two instances of the Reaper in parallel. The helm values for the notification reaper need to specify:

```sql
fullnameOverride: "notification-reaper"
app:
  reaperType: "ttn"
```

and the helm values for the deletion reaper need to specify:

```sql
fullnameOverride: "deletion-reaper"
app:
  reaperType: "ttl"
```

