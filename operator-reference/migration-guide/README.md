---
description: Explanation of migration strategies between Granary platform versions.
---

# Migration guide

## Upgrade from Granary 1.0 "Aretha" to Granary 1.1 "Cliff"

### Profile Updater breaking changes

Updating the Profile Updater entails updating the [database table "profilestore"](../../developer-reference/dataflow/profile-store/#table-profilestore) because with Granary 1.1 the database table "profilestore" is now under control of Flyway within the Profile Updater. The following migrations will be carried out:

```sql
ALTER TABLE public.profilestore ADD COLUMN ttn VARCHAR NOT NULL DEFAULT 'P100Y';

CREATE INDEX IF NOT EXISTS profilestore_time_to_notify ON public.profilestore (profile_time_to_act(inserted, ttn));
```

For your use cases, you need to additionally run these `UPDATE` queries manually:

```sql
UPDATE public.profilestore SET ttn = ttl WHERE ttn <> ttl;
UPDATE public.profilestore SET ttl = 'P100Y' WHERE ttl <> 'P100Y';
```

This means, the content of the `ttl`-column will be copied to the `ttn`-column \(line 1\) while the `ttl`-column will be reset to the default value `'P100Y'` \(line 2\). This is due to semantic changes of `ttl`-expiry \(from notification to actual deletion\). Before upgrading the Profile Updater, you should stop all belts that are setting `ttl`  to values other than `'P100Y'`. Otherwise, be aware of the semantic changes within the history of your data.

#### After upgrading the Profile Updater, before restarting the belts

**Option 1:** If your use case wants the new \(deleting\) `ttl` semantics to be applied to older grains written by an existing belt, a manual copying of the values needs to be done using the `origin`-column before restarting the belt.

```sql
UPDATE public.profilestore SET ttl = ttn WHERE origin = /belts/<belt-id>;
```

**Option 2:** If you want the new \(notifying\) `ttn`-semantics to be applied to future grains, the belt needs to be updated to the new extractor version. The belt code needs to be adapted, in case the `ttl` is set explicitly using the parameter keyword. See [below](./#belt-callback-breaking-changes).

### Additional Reaper deployment

After upgrading the Profile Updater, please ensure to upgrade your existing Reaper deployment to the latest version. Additionally to that, add a second deployment of the Reaper's latest version. However, this instance needs to be deployment with the Helm property `app.reaperType: "ttn"`. For details, see [Reaper 1.1](reaper-1.1.md).

### Belt Extractor breaking changes

Please do **not** upgrade ****the Belt Extractor without having upgraded the profile updater.

### Belt Callback breaking changes

The notification and deletion mechanics of the platform underwent a major refactoring. Belts reading Granary 1.0 TTL event types will need to be adapted as follows:

**Belts using notifications:** Notifications are now handled separately by the platform using TTN event types. If previously you wanted to act on notifications of the event type topic `grnry_ttl_myprofiletype`, you will now need to change your belt to read from `grnry_ttn_myprofiletype-ttn`. To trigger notifications you need to alter a grains `ttn` by using the profile updaters new `set_ttn` operation.

**Belts using deletions:** Deletions are now handled by the platform itself. Thus **your belt callback** **does not need to send delete operations** to the profile updater. If you want to act on the deletion of a grain your belt now needs to read from `grnry_ttl_myprofiletype-ttl`. To change when a grain gets deleted you need to alter the grains `ttl` by using the profile updaters new `set_ttl` operation.

For additional details and examples, see [Belt Extractor 1.1](belt-extractor-1.1.md).

**That's it.**

