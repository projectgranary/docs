---
description: 'https://gitlab.alvary.io/grnry/kafka-profile-update'
---

# Profile Store

![](../../.gitbook/assets/profilestore.png)

## Table `'profilestore'`

{% tabs %}
{% tab title="Spec" %}
| Key | Description |
| :--- | :--- |
| **id**  | Correlation ID. Groups all events that belong to the same tracking entity \(cookie, device, customer, contact, claim, etc.\). This is identical to the correlation-id in the Event Store. |
| **j** | A nested data structure that complies with [https://gitlab.alvary.io/grnry/kafka-profile-update/blob/master/PROFILESPECS.md\#profile-specification](https://gitlab.alvary.io/grnry/kafka-profile-update/blob/master/PROFILESPECS.md#profile-specification) |
{% endtab %}

{% tab title="Example" %}
```javascript
{
  "_id": "0815",
  "a1": {
    "_latest": {
      "_v": "abc",
      "_c": 0.4,
      "_in": 123,
      "_ttl": "P100Y",
      "_origin": null,
      "_reader": "_all"
    }
  },
  "a2": {
    "_schema": "default_1",
    "b1": {
      "2018-09-21": {
        "_v": "21",
        "_c": 0.1,
        "_ttl": "P100Y",
        "_origin": null,
        "_reader": "_all"
      },
      "2018-09-22": {
        "_v": "22",
        "_c": 0.2,
        "_ttl": "P100Y",
        "_origin": null,
        "_reader": "_all"
      },
      "_latest": {
        "_v": "23",
        "_in": "2018-09-23",
        "_c": 0.3,
        "_ttl": "P100Y",
        "_origin": null,
        "_reader": "_all"
      }
    },
    "b2": {
      "_latest": {
        "_v": "123456",
        "_c": 0.4,
        "_in": "2018-09-20",
        "_ttl": "P100Y",
        "_origin": null,
        "_reader": "_all"
      }
    }
  },
  "a3": {
    "_schema": "foo",
    "b3": {
      "_schema": "bar",
      "c3": {
        "_schema": "baz",
        "d3": {
          "_schema": "fool",
          "e3": {
            "_latest": {
              "_v": "123456",
              "_c": 0.5,
              "_in": "2018-09-20",
              "_ttl": "P100Y",
              "_origin": null,
              "_reader": "_all"
            }
          }
        }
      }
    }
  }
}

```
{% endtab %}
{% endtabs %}

