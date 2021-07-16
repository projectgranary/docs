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

