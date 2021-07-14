---
description: Migration Guide
---

# Belt API 1.1

## Upgrading Belt API

Granary 1.1 Cliff enables users to deploy Custom Belts via the Belt API. With the introduction of [Flyway](https://flywaydb.org/) the API will migrate its tables automatically on startup.

If you decided to provide your own Docker Images, or mirror ours, you will have to update all Belts in the Database with this SQL statement.

```sql
BEGIN;
UPDATE belts SET image = '<your image name>', image_pull_secret = '<your pull secret>';
-- COMMIT;
```

{% hint style="info" %}
This will not require a restart of currently running Belts.
{% endhint %}



