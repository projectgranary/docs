---
description: Explanation of migration strategies between Granary platform versions.
---

# Migration guide

## Upgrade from Granary 0.9.1 "Marie" to Granary 1.0 "Aretha"

### Update Granary Components

Update all remaining Granary components to their Granary 0.9 version as denoted in the [release notes](../granary-release-notes/). Bold versions indicate an update.

### Belt Callback breaking changes

See subpage [Belt Extractor 1.0](belt-extractor-1.0.md).

### Belt-API breaking changes

The Belt-API now uses a different logic for assigning IDs to the belts. For that reason please make sure that there exists an entry in the 'belts\_log' table for each belt in the 'belts' table. If this is not the case, you can create the missing entries by running this SQL snippet:

```sql
INSERT INTO belts_log (belt_id, name, description, labels, affected_paths,
    replicas, event_types, millicpu, millicpu_requests, memory, memory_requests, author, reader, editor, viewer,
    assumed_role,  requirements_py, extractor_version, extractor_fn, belt_type,
    runtime, parameter, debug, fetch_profile, secret, secret_username, secret_password, partition_offsets, kafka_destination_topic,
    username, action, version, created, profile_type, volumes, volume_mounts, extra_envs)
    SELECT id, name, description, labels, affected_paths,
        replicas, event_types, millicpu, millicpu_requests, memory, memory_requests, author, reader, editor, viewer,
        assumed_role,  requirements_py, extractor_version, extractor_fn, belt_type,
        runtime, parameter, debug, fetch_profile, secret, secret_username, secret_password, partition_offsets, kafka_destination_topic, 
			  username, 'INSERT', VERSION, created, profile_type, volumes, volume_mounts, extra_envs
    FROM belts
    ON CONFLICT DO NOTHING;
```

**That's it.**

