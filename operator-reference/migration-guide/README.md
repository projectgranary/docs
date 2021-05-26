---
description: Explanation of migration strategies between Granary platform versions.
---

# Migration guide

## Upgrade from Granary 1.0 "Aretha" to Granary 1.1 "Cliff"

### Profile Updater breaking changes

Updating the Profile Updater entails updating the [database table "profilestore"](../../developer-reference/dataflow/profile-store/#table-profilestore) because with Granary 1.1 the database table "profilestore" is now under control of Flyway within the Profile Updater. The following migrations will be carried out:

```sql
ALTER TABLE public.profilestore ADD COLUMN ttn VARCHAR NOT NULL DEFAULT 'P100Y';
```

Before upgrading the Profile Updater, you should stop all belts that are currently setting `ttl`  to values other than `'P100Y'`. Otherwise, be aware of the semantic changes within the history of your data.

#### After upgrading the Profile Updater, before restarting the belts

As the new `ttn` column is initialized with `P100Y`, you need to run additionally `UPDATE` queries manually **if** your use case wants to continue to use notification feature. Otherwise those grains will now be automatically deleted once the `ttl` expires.

**Step 1:** Run these `UPDATE` queries with `WHERE` clauses that only include your use case's grains. E.g.:

```sql
UPDATE public.profilestore SET ttn = ttl WHERE profile_type='<your type>' AND origin = '<your origin>';
UPDATE public.profilestore SET ttl = 'P100Y' WHERE profile_type='<your type>' AND origin = '<your origin>';
```

This means, the content of the `ttl`-column will be copied to the `ttn`-column \(line 1\) while the `ttl`-column will be reset to the default value `'P100Y'` \(line 2\). This is due to semantic changes of `ttl`-expiry \(from notification to actual deletion\). 

**Step 2:** If you want to continue using the notification semantics rather than deletion, the belt needs to be updated to the latest extractor version. The belt code needs to be adapted, in case the `ttl` is set explicitly using the parameter keyword. See [below](./#belt-extractor-breaking-changes).

**Step 3**: Restart the Belts.



### Additional Reaper deployment for Notification

After upgrading the Profile Updater, please ensure to upgrade your existing Reaper deployment to the latest version. Additionally to that, add a second deployment of the Reaper's latest version. However, this instance needs to be deployed with the Helm property `app.reaperType: "ttn"` and an index needs to be created before deployment. For details, see [Reaper 1.1](reaper-1.1.md). 



### Belt Extractor breaking changes

Please do **not** upgrade ****the Belt Extractor without having upgraded the Profile Updater.

The notification and deletion mechanics of the platform underwent a major refactoring. Belts reading Granary 1.0 TTL event types will need to be adapted as follows:

**Belts using notifications:** Notifications are now handled separately by the platform using TTN event types. If previously you wanted to act on notifications of the event type topic `grnry_ttl_myprofiletype`, you will now need to change your belt to read from `grnry_ttn_myprofiletype-ttn`. To trigger notifications you need to alter a grains `ttn` by using the profile updaters new `set_ttn` operation.

**Belts using deletions:** Deletions are now handled by the platform itself. Thus **your belt callback** **does not need to send delete operations** to the profile updater. If you want to act on the deletion of a grain your belt now needs to read from `grnry_ttl_myprofiletype-ttl`. To change when a grain gets deleted you need to alter the grains `ttl` by using the profile updaters new `set_ttl` operation.

For additional details and examples, see [Belt Extractor 1.1](belt-extractor-1.1.md).



### Deployment of Central Deletion Belt

Next to the changes in the ttl / ttn handling in the belts, Granary 1.1 ships with a central deletion belt which automatically deletes all Grains from Profile Store that were reaped by Profile Store's ttl reaper. To deploy this deletion belt, use this helm chart: [Central Deletion Belt](../installation/with-helm/central-deletion-belt.md). To understand the Profile Store deletion mechanics, head over to [Reaping of Grains for Notification or Deletion](../../developer-reference/dataflow/profile-store/reaper.md).



### SCDF App "Eventstore Batch Sink" changes

From Granary 1.1 on the "[eventstore](../../developer-reference/dataflow/event-store/#table-eventstore)" database table will be under Flyway version control within the Eventstore Batch Sink. Hence, updating the Eventstore Batch Sink \(which is included in the SCDF Apps\) entails updating the "eventstore" table. The following migrations will be carried out once a Granary 1.1 Eventstore Batch Sink \(Persister\) is started:

```sql
ALTER TABLE public.eventstore ADD COLUMN ttl VARCHAR;
CREATE OR REPLACE FUNCTION event_time_to_act(created bigint, ttl varchar) RETURNS bigint AS $value$ SELECT created/1000 + cast(extract(epoch from ttl::interval) as bigint); $value$ LANGUAGE SQL IMMUTABLE;
```

After this migration and before the first run of the [Event Store Reaper](../../developer-reference/dataflow/event-store/deletion-of-raw-events.md), you must add an index with the following statement to improve the Event Store Reaper performance:

```sql
 CREATE INDEX IF NOT EXISTS eventstore_time_to_act ON public.eventstore (event_time_to_act(created, ttl));
```

To understand the Event Store deletion mechanics, head over to [Deletion of Raw Events](../../developer-reference/dataflow/event-store/deletion-of-raw-events.md).

### 

### Update Granary Components

Update all remaining Granary components to their Granary 1.1 version as denoted in the [release notes](../granary-release-notes/). Bold versions indicate an update.

**That's it.**

