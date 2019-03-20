---
description: Describes the Profile Store and Profile Update mechanism
---

# Profile Store

![](../../.gitbook/assets/profilestore.png)

The profile store holds integrated data for entities in so-called profiles. Such entities can be customers, customers' contracts or customers' behavior, etc. The profile store holds a profile for each correlation id \[and profile type \(implemented in the near future\). A profile type denotes the abstract notion of an entity, such as customer, contract, or behavior\].

A profile is a JSON document. A profile consists of fragments and grains. Fragments group other fragments and grains. Grains store the information at different points in time. Grain values carry the actual value and respective meta information.

[https://gitlab.alvary.io/grnry/kafka-profile-update/blob/master/PROFILESPECS.md\#profile-specification](https://gitlab.alvary.io/grnry/kafka-profile-update/blob/master/PROFILESPECS.md#profile-specification)

## Table `profilestore`

The profile store is a distributed table in a database. Each tuple in that table represents some profile's grain at some point in time. A query to the `profilestore` table therefore retrieves a single grain value's information. The full nested JSON data structure that complies with above linked specification must be pulled via the Profile API.

`profilestore` table tuples consist of the following fields.

{% tabs %}
{% tab title="Spec" %}
<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Data Type</th>
      <th style="text-align:left">Default</th>
      <th style="text-align:left">Null</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>correlation_id </b>
      </td>
      <td style="text-align:left">Correlation ID. Groups all grains that belong to the same profile (cookie,
        device, customer, contact, claim, etc.). This is identical to the correlation-id
        in the Event Store.</td>
      <td style="text-align:left">varchar</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><code>NOT NULL</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>profile_type</b>
      </td>
      <td style="text-align:left">Profile type. Describes the type of the profile.</td>
      <td style="text-align:left">varchar</td>
      <td style="text-align:left"><code>_d</code>
      </td>
      <td style="text-align:left"><code>NOT NULL</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>path</b>
      </td>
      <td style="text-align:left">Path. Denotes the grain&apos;s path in the profile&apos;s JSON data structure.
        The last past element defines the grain&apos;s name.</td>
      <td style="text-align:left">varchar</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><code>NOT NULL</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>pit</b>
      </td>
      <td style="text-align:left">Point in Time. Captures the history of grains.</td>
      <td style="text-align:left">varchar</td>
      <td style="text-align:left"><code>_latest</code>
      </td>
      <td style="text-align:left"><code>NOT NULL</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">value</td>
      <td style="text-align:left">Value. Stores the grain&apos;s value. Must be a JSON string or JSON array
        of strings.</td>
      <td style="text-align:left">jsonb</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><code>NOT NULL</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">certainty</td>
      <td style="text-align:left">Certainty. Describes the grain value&apos;s certainty.</td>
      <td style="text-align:left">real</td>
      <td style="text-align:left"><code>1.0</code>
      </td>
      <td style="text-align:left"><code>NOT NULL</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">grain_type</td>
      <td style="text-align:left">
        <p>Grain type. Describes the grain&apos;s type. Naming convention:</p>
        <ul>
          <li>t == text</li>
          <li>a == array</li>
          <li>c == counter</li>
        </ul>
      </td>
      <td style="text-align:left">character</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><code>NOT NULL</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">inserted</td>
      <td style="text-align:left">Inserted. States the unix epoch timestamp in milliseconds when the grain
        was first inserted.</td>
      <td style="text-align:left">bigint</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><code>NOT NULL</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ttl</td>
      <td style="text-align:left">Time to life. Allows to delete the grain after a defined duration. Format
        is <a href="https://en.wikipedia.org/wiki/ISO_8601#Durations">https://en.wikipedia.org/wiki/ISO_8601#Durations</a>.</td>
      <td
      style="text-align:left">varchar</td>
        <td style="text-align:left"><code>P100Y</code>
        </td>
        <td style="text-align:left"><code>NOT NULL</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">reader</td>
      <td style="text-align:left">Reader. Authorizes the access to the grain. By default access is granted
        to all authenticated users.</td>
      <td style="text-align:left">varchar</td>
      <td style="text-align:left"><code>_auth</code>
      </td>
      <td style="text-align:left"><code>NOT NULL</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">origin</td>
      <td style="text-align:left">Origin. Allows to trace the profile&apos;s origin.</td>
      <td style="text-align:left">varchar</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">-</td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Example" %}
```javascript
// retrieved via Profile API
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
    "b3": {
      "c3": {
        "d3": {
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

## Component `Profile Updater`

Belts process input and create Profile Updates. The Profile Updater merges such Profile Updates into the Profile Store.  These Profile Updates must follow the specification in [https://gitlab.alvary.io/grnry/kafka-profile-update/blob/master/PROFILESPECS.md\#profile-update-specification](https://gitlab.alvary.io/grnry/kafka-profile-update/blob/master/PROFILESPECS.md#profile-update-specification). Currently there are three operations:

* `_set` inserts the current garin value at \_latest and overwrites if it already exists.
* `_set_with_history` inserts the current grain value at \_latest and stores previous \_latest grain value at its insert point in time if it already exists.
* `_delete` deletes the \_latest grain value at the specified path.
* `_inc` creates or increments a counter.

Each Profile Update carries the the grain value to be merged into the store. The grain values consists of the actual value, denoted as `_v`, and its meta information. \(Currently\) `_v` must be a JSON string or a JSON array of strings.

## Counter

`_inc` operation grain values must be of the form: `initialvalue|stepsize|steps`, e.g., `0|1|1`. This creates or increments counter grain values like `{"_initial":0, "_step":1, "_current":1}`. Please see the above linked spec. 

`initialvalue` defines the counter's start.  `stepsize` defines the width of each counter increment/decrement. `steps` is the number of increments/decrements to do. All three are real numbers, e.g., `10|0.5|-1` defines a   single-step decrement of width 0.5 for the counter starting at 10.

See [https://gitlab.alvary.io/grnry/kafka-profile-update](https://gitlab.alvary.io/grnry/kafka-profile-update)

