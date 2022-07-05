---
description: Migration from earlier versions to 1.2
---

# Belt API 1.2

### New Property `app.beltImageTag`

On earlier versions of Granary the default version for the Belt Extractor image could be set by adding the extra environment variable `BELT_EXTRACTOR_VERSION` in the `extra.extraEnv` section of the Belt API's `values.yaml` file. Now, Ganary 1.2 Dolores enables users to specify a default value for the Belt Extractor version in an explicit property in the helm `values.yaml` file.

{% hint style="danger" %}
If the variable is set under both sections `extra.extraEnv` and `app.beltImageTag`, the `extraEnv` will take precedence.
{% endhint %}

{% code title="beltApiValues.yaml" %}
```yaml
app:
  beltImageTag: 2.0.0
  
# make sure to remove the extra env
extra:
  extraEnv:
#   - name: BELT_EXTRACTOR_VERSION
#     value: latest
          
```
{% endcode %}

### Updated Value Range for fetchProfile Belt Property

{% hint style="danger" %}
Value range of fetchProfile changed from \["TRUE", "FALSE", "LAZY"] to \["PREFETCH", "PREFETCH\_WITH\_HISTORY", "FALSE", "LAZY"] for Belt Images newer than 2.0.0.
{% endhint %}

To achieve a behavior equal to `fetchProfile="TRUE"` in older versions, set it to `"PREFETCH_WITH_HISTORY"`. This will prefetch the profile containing all data, including historical grains (not recommended). Otherwise, on `fetchProfile="PREFETCH"`, the profile will be prefetched as well, but will contain the latest grains only (recommended). Setting `fetchProfile="LAZY"` will force you to assign a value to the `withHistory` parameter on a `profileClient.getProfile()` method call.
