---
description: Migration guide to upgrade to Belt Extractor 1.0
---

# Belt Extractor 1.0

With the Release of Granary 1.0 Aretha the Belt Extractor received extensive refactoring. The basic functionality of the Belt Extractor is now provided as a pip package with the name `grnry-belt` . This allows users to test custom belt callback code locally. For more see [Local Testing](../../learning-grnry-1/using-data-in-granary/best-practices/local-testing.md) .

As a result of this refactoring import paths changed and current callbacks have to be updated if the `extractorVersion` is upgraded to 1.0 . 

#### Custom Belt Callback with 0.9 Marie

```python
from grnry.beltextractor.update import Update

def execute(event_headers, event_payload, profile=None):
    ....
    return [update]
```

#### Custom Belt Callback with 1.0 Aretha

```python
def execute(event_headers, event_payload, profile=None):
    ...
    return [update]
```

{% hint style="info" %}
The **Update** object is injected into the callback and does **not** require an additional import statement.
{% endhint %}

Also if you use the option to provide a default extractor function in the Belt API, don't forget to update that as well.

