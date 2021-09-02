---
description: This page specifies the configuration of segments in Granary.
---

# Segment Table Creation

## Segment Creation Job

This component generates so-called segments, which can be understood as relational view on Profile Store or Event Store data. 

Depending on the segment's access pattern, segments can be **views** or **tables:**

* **View segments** are always up-to-date as queries are run at access time against Profile Store / Event Store.
* **Table segments** are updated based on a cron schedule, are pre-computed, and can be equipped with additional indexes. During the scheduled pre-computation, the old segment is still fully accessible.

The available storage layers are:

* **PostgreSQL** \(compatible with Amazon Aurora - allowing to leverage read replicas for view segments\)
* **Citus** Data PostgreSQL

Using standard **PostgreSQL**, table segments are normal database tables. Using **Citus**, table segments are distributed tables, co-located with distributed Citus source tables. Targets are populated per shard. Thus, network load is very low and generation takes place in parallel. 

Currently, three different segment generator types are available:

* **Pivot**, which generates tuples from a table comprising key/value-like tuples
* **Generic**, which builds a filtered version of the input table
* **Flexible**, which takes a user-given query to create its segment

{% hint style="warning" %}
Please note that due to technical limitations, `pivot`segments might fail if many grains \(~5.000.000\) are being selected and may even cause database restarts.
{% endhint %}

## Configure

The creation job runs in a container and can be deployed via [Segment Management API](). The following table comprises a complete list of variables \(required if there is no default\):

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>TYPE</code>
      </td>
      <td style="text-align:left">type of generator, <code>pivot</code> , <code>generic</code> or <code>flexible</code>
      </td>
      <td style="text-align:left"><code>pivot</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DB_TYPE</code>
      </td>
      <td style="text-align:left">storage-layer type, <code>citus</code> or <code>postgres</code>
      </td>
      <td style="text-align:left"><code>postgres</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DB_USE_VIEWS</code>
      </td>
      <td style="text-align:left">flag indicating if generated segment should be a view</td>
      <td style="text-align:left"><code>false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DB_PRESERVE_VIEWS</code>
      </td>
      <td style="text-align:left">
        <p>Segments are recreated based on a CRON schedule. If a custom-made view
          relies on a Segment, it is dropped and needs to be recreated after update.
          With this flag set to <code>true</code>, manually created views on the Segment
          are preserved.</p>
        <p>WARNING: With this feature Segment columns can only be added and not be
          changed. On breaking change of the segment, just run the segment job with
          this flag disabled and enable it again afterwards.</p>
      </td>
      <td style="text-align:left"><code>false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DB_HOST</code>
      </td>
      <td style="text-align:left">database endpoint (citus master host or aurora writer endpoint)</td>
      <td
      style="text-align:left"><code>grnry-pg</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DB_PORT</code>
      </td>
      <td style="text-align:left">database port</td>
      <td style="text-align:left"><code>5432</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DB_USER</code>
      </td>
      <td style="text-align:left">database user name</td>
      <td style="text-align:left">Secret Reference needed</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DB_PASSWORD</code>
      </td>
      <td style="text-align:left">database user password</td>
      <td style="text-align:left">Secret Reference needed</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DB_NAME</code>
      </td>
      <td style="text-align:left">name of the database</td>
      <td style="text-align:left">Secret Reference needed</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DB_USE_SSL</code>
      </td>
      <td style="text-align:left">whether to enforce SSL for DB connection</td>
      <td style="text-align:left"><code>true</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>SOURCE_SCHEMA_NAME</code>
      </td>
      <td style="text-align:left">name of schema of source table</td>
      <td style="text-align:left"><code>public</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>SOURCE_TABLE_NAME</code>
      </td>
      <td style="text-align:left">name of source table</td>
      <td style="text-align:left"><code>profilestore</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>TARGET_SCHEMA_NAME</code>
      </td>
      <td style="text-align:left">name of schema for target segment</td>
      <td style="text-align:left"><code>segments</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>TARGET_SEGMENT_NAME</code>
      </td>
      <td style="text-align:left">name for target segment, if already existing old segment will be overwritten</td>
      <td
      style="text-align:left"><code>profilestore_seg</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>COLUMN_PLACEHOLDER</code>
      </td>
      <td style="text-align:left">placeholder for the path name to be used in the pivot transformation function</td>
      <td
      style="text-align:left"><code>?</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>TYPE_SEPARATOR</code>
      </td>
      <td style="text-align:left">separator between pivot transformation function and result type</td>
      <td
      style="text-align:left"><code>::</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DEFINITION_SEPARATOR</code>
      </td>
      <td style="text-align:left">separator between name and transformation</td>
      <td style="text-align:left"><code>=</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>TRANSFORMATION_SEPARATOR</code>
      </td>
      <td style="text-align:left">separator between multiple transformations, e.g. <code>body_txt=message-&gt;&apos;body&apos;::text|body_json=replace(message-&gt;&apos;body&apos;#&gt;&gt;&apos;{}&apos;, &apos;\&apos;, &apos;&apos;)::jsonb</code>
      </td>
      <td style="text-align:left"><code>|</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>CITUS_DIST_COL</code>
      </td>
      <td style="text-align:left">Name of the column the source table is and the target segment will be
        distributed by. Currently, this has to be a single column and is only mandatory
        if <code>DB_TYPE=citus</code>.</td>
      <td style="text-align:left">correlation_id</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DEBUG</code>
      </td>
      <td style="text-align:left">whether to print all database statement and responses</td>
      <td style="text-align:left"><code>true</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>SOURCE_WHERE_CLAUSE</code>
      </td>
      <td style="text-align:left">where clause as SQL (without &quot;WHERE&quot; itself), must not be empty.</td>
      <td
      style="text-align:left"><code>&quot;pit=&apos;_latest&apos;&quot;</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>PROMETHEUS_PUSHGATEWAY</code>
      </td>
      <td style="text-align:left">address of gateway to push prometheus metrics, format <code>&apos;&lt;host&gt;:&lt;port&gt;&apos;</code>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><code>PROMETHEUS_JOB</code>
      </td>
      <td style="text-align:left">job name for pushgateway, the metrics will be grouped by this name.</td>
      <td
      style="text-align:left"><code>segmentcreation</code>
        </td>
    </tr>
  </tbody>
</table>

### Segment Indexes

There are parameters to define **indexes** on the resulting segment. Note that creating indexes is not possible on segments that are generated as views.

| Parameter | Description | Default |
| :--- | :--- | :--- |
| `TARGET_SEGMENT_INDEX_SEPARATOR` | Separator between index definitions | \| |
| `TARGET_SEGMENT_INDEXES` | Defines index name and expression. Must be in format `<name>=<index expression>`.  For example: `segidx1=(event_id)` or `segidx2= USING gin (body_json, headers)` |  |

### Segment Views

There are parameters to define **views** on the resulting segment. These views can be used to enforce access regulations. Consider profiles with an `/isLocked` grain set to `true` or `false` as an example. Then, a segment on such profile can contain that grain, which is further used define two complementary views -- one containing _unlocked_ profiles and a second containing _locked_ profiles.

For this example, the respective configuration could look as follows:

`PIVOT_PATHS: "/pathA,/pathB,/pathC,/isLocked"    
PIVOT_TRANSFORMATIONS: "/isLocked= ?#>>'{}'::text"    
TARGET_SEGMENT_VIEWS: "locked:\"/isLocked\" = 'true',unlocked:\"/isLocked\" = 'false'"`

Detailed configuration

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>TARGET_SEGMENT_VIEW_SEPARATOR</code>
      </td>
      <td style="text-align:left">Separator between view definitions</td>
      <td style="text-align:left"><code>,</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>TARGET_SEGMENT_VIEW_DEFINITION_SEPARATOR</code>
      </td>
      <td style="text-align:left">Separator between view <em>name</em> and view <em>condition</em>
      </td>
      <td style="text-align:left"><code>:</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>TARGET_SEGMENT_VIEWS</code>
      </td>
      <td style="text-align:left">
        <p><code>TARGET_SEGMENT_VIEW_SEPARATOR</code> separated list auf views.</p>
        <p>Definition of a view: <em>name definition-separator condition</em>
        </p>
      </td>
      <td style="text-align:left">-</td>
    </tr>
  </tbody>
</table>

One can now set access to the resulting views, containing either unlocked or locked profiles, via Granary's [IAM control](../../../operator-reference/identity-and-access-management/).

### Generic Generator

The generic generator creates a filtered projection from the source table. Data is selected using the specified where clause.

Also, there are **generic generator** specific variables as specified below.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>GENERIC_COLUMNS</code>
      </td>
      <td style="text-align:left">source columns to be used without transformation</td>
      <td style="text-align:left">event_id</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>GENERIC_TRANSFORMATIONS</code>
      </td>
      <td style="text-align:left">columns to be created from transformation. must be in format <code>&lt;name&gt;&lt;definition_separator&gt;&lt;sql expression&gt;&lt;type separator&gt;&lt;type&gt;</code>
      </td>
      <td style="text-align:left">
        <p><code>body_txt=</code>
        </p>
        <p><code>message-&gt;&apos;body&apos;::text|</code>
        </p>
        <p><code>body_json=</code>
        </p>
        <p><code>replace(message-&gt;&apos;body&apos;#&gt;&gt;&apos;{}&apos;, &apos;\&apos;, &apos;&apos;)::jsonb|</code>
        </p>
        <p><code>headers=</code>
        </p>
        <p><code>message-&gt;&apos;headers&apos;::jsonb</code>
        </p>
      </td>
    </tr>
  </tbody>
</table>

### Pivot Generator

Additionally, there are **pivot generator** specific variables as specified below.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>PIVOT_PATHS</code>
      </td>
      <td style="text-align:left">list of paths to be turned into columns, comma-separated, no explicit
        spec means all paths</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><code>PIVOT_ALIASES</code>
      </td>
      <td style="text-align:left">list of aliases for the paths to be turned into columns, comma-separated</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><code>PIVOT_TRANSFORMATIONS</code>
      </td>
      <td style="text-align:left">
        <p>list of transformations to apply on paths;</p>
        <p>must be in the format <code>&lt;path&gt;&lt;definition_separator&gt;&lt;sql expression with placeholder&gt;&lt;type separator&gt;&lt;result type&gt;</code>
        </p>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><code>PIVOT_ALLOW_EMPTY</code>
      </td>
      <td style="text-align:left">whether to remove empty rows from the segment output</td>
      <td style="text-align:left"><code>true</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>PIVOT_SEGMENT_WHERE_CLAUSE</code>
      </td>
      <td style="text-align:left">where clause as SQL (without <code>WHERE</code> itself) to filter segments
        after segmentation, e.g., <code>a::text = &apos;\&quot;foo\&quot;&apos;</code> (note
        that column values are of type <code>jsonb</code>)</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

The pivot generator presupposes that the following input columns are present in the source table.

`<CITUS_DIST_COL>` \(varchar, only necessary if `DB_TYPE=citus`\)

`path` \(text/varchar\)

`value` \(jsonb\)

As an example see the ProfileStore:

```text
correlation_id varchar NOT NULL,
profile_type varchar NOT NULL DEFAULT '_d',
path varchar NOT NULL,
pit varchar NOT NULL DEFAULT '_latest',
value jsonb NOT NULL,
certainty real NOT NULL DEFAULT 1.0,
grain_type character NOT NULL,
inserted bigint NOT NULL,
ttl varchar NOT NULL DEFAULT 'P100Y',
ttn varchar NOT NULL DEFAULT 'P100Y',
reader varchar NOT NULL DEFAULT '_auth',
origin varchar,
PRIMARY KEY(id, profile_type, path, pit)
```

For each `<CITUS_DIST_COL>`_/_`path` combination there should be returned a single row whose "`value`"-column is used. Make sure this assumption holds, e.g., via respective where clauses.

#### Pivot Example

Consider the following input table.

<table>
  <thead>
    <tr>
      <th style="text-align:left">correlation_id</th>
      <th style="text-align:left">profile_type</th>
      <th style="text-align:left">path</th>
      <th style="text-align:left">pit</th>
      <th style="text-align:left">value</th>
      <th style="text-align:left">certainty</th>
      <th style="text-align:left">grain_type</th>
      <th style="text-align:left">inserted</th>
      <th style="text-align:left">ttl</th>
      <th style="text-align:left">ttn</th>
      <th style="text-align:left">reader</th>
      <th style="text-align:left">origin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">_d</td>
      <td style="text-align:left">/path1</td>
      <td style="text-align:left">_latest</td>
      <td style="text-align:left">foo</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">t</td>
      <td style="text-align:left">7</td>
      <td style="text-align:left">
        <p></p>
        <p>p100y</p>
      </td>
      <td style="text-align:left">p1d</td>
      <td style="text-align:left">_auth</td>
      <td style="text-align:left">test</td>
    </tr>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">_d</td>
      <td style="text-align:left">/path2</td>
      <td style="text-align:left">_latest</td>
      <td style="text-align:left">bar</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">t</td>
      <td style="text-align:left">8</td>
      <td style="text-align:left">
        <p></p>
        <p>p100y</p>
      </td>
      <td style="text-align:left">p1d</td>
      <td style="text-align:left">_auth</td>
      <td style="text-align:left">test</td>
    </tr>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">_d</td>
      <td style="text-align:left">/path3</td>
      <td style="text-align:left">_latest</td>
      <td style="text-align:left">baz</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">t</td>
      <td style="text-align:left">9</td>
      <td style="text-align:left">
        <p></p>
        <p>p100y</p>
      </td>
      <td style="text-align:left">p1d</td>
      <td style="text-align:left">_auth</td>
      <td style="text-align:left">test</td>
    </tr>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">pt</td>
      <td style="text-align:left">/path3</td>
      <td style="text-align:left">_latest</td>
      <td style="text-align:left">bazz</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">t</td>
      <td style="text-align:left">9</td>
      <td style="text-align:left">
        <p></p>
        <p>p100y</p>
      </td>
      <td style="text-align:left">p1d</td>
      <td style="text-align:left">_auth</td>
      <td style="text-align:left">test</td>
    </tr>
    <tr>
      <td style="text-align:left">1</td>
      <td style="text-align:left">pt</td>
      <td style="text-align:left">/path4</td>
      <td style="text-align:left">_latest</td>
      <td style="text-align:left">3</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">t</td>
      <td style="text-align:left">9</td>
      <td style="text-align:left">
        <p></p>
        <p>p100y</p>
      </td>
      <td style="text-align:left">p1d</td>
      <td style="text-align:left">_auth</td>
      <td style="text-align:left">test</td>
    </tr>
    <tr>
      <td style="text-align:left">2</td>
      <td style="text-align:left">pt</td>
      <td style="text-align:left">/path3</td>
      <td style="text-align:left">_latest</td>
      <td style="text-align:left">bazz</td>
      <td style="text-align:left">1</td>
      <td style="text-align:left">t</td>
      <td style="text-align:left">9</td>
      <td style="text-align:left">
        <p></p>
        <p>p100y</p>
      </td>
      <td style="text-align:left">p1d</td>
      <td style="text-align:left">_auth</td>
      <td style="text-align:left">test</td>
    </tr>
  </tbody>
</table>

Consider the following configuration.

```text
- name: TYPE
  value: "pivot"
- name: CITUS_DIST_COL
  value: "correlation_id"
- name: WHERE_CLAUSE
  value: "profile_type='_d' and pit='_latest' and reader='_auth'"
- name: PIVOT_PATHS
  value: "/path1,/path2,/path3"
```

This results in the following output containing _\_latest_ values for path _a_, _b_ and _c_ per _correlation\_id_.

| correlation\_id | /path1 | /path2 | /path3 |
| :--- | :--- | :--- | :--- |
| 1 | foo | bar | baz |
| 2 | null | null | bazz |

Note that if _PIVOT\_PATHS_ is empty, then it uses all available paths. Determining all path is a potentially long-running operation.

If filtering by paths \(e.g. /path1 in the example above\) the result might contain empty rows:

| correlation\_id | /path1 |
| :--- | :--- |
| 1 | foo |
| 2 | null |

These can be suppressed by setting _ALLOW\_EMPTY_ to _False_. By defining a where clause in _SEGMENT\_FILTER\_CLAUSE_ the output can be further filtered.

#### Transformations of path content

When creating the pivot table you have the option to specify arbitrary transformations on specific paths. These have to be valid sql expressions with a placeholder for the path name. You also have to add the data type of the transformation's result. The resulting column in the pivot table will have this data type.

E.g. `/path1=? #>> a::text` will select the element `a` in the jsonb object of `/path1` as a text column.

The placeholders and separators \(`=`, `?` and `::`\) can be customized if they would otherwise conflict with special characters in the sql expression. Note that all values are initially of type `jsonb`!

Below you can see a full example:

Source table:

| correlation\_id | profile\_type | path | pit | value | ... |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | \_d | /path1 | \_latest | 20 | ... |
| 2 | pt | /path1 | \_latest | 30 | ... |
| 3 | pt | /path1 | \_latest | 60 | ... |

Configuration:

```text
env:
- name: PIVOT_TRANSFORMATIONS
  value: /path1=(CAST((? #> '{}') AS integer) > 50)::boolean
```

Result:

| correlation\_id | /path1 |
| :--- | :--- |
| 1 | false |
| 2 | false |
| 3 | true |

Common transformations could be:

* Selecting a jsonb path as jsonb object: `/path1=? #> a::jsonb`
* Selecting a jsonb path as sql text object: `/path1=? #>> a::text`
* Selecting an element from a jsonb array: `/path1=(? #> arr)->1::text`
* Converting to a different type: `/path1=CAST((? #> '{}') AS DATE)::date` - `#> '{}'` selects the root element of a jsonb object
* Computing a boolean expression: `/path1=CAST((? #> '{}') AS int) > 30`

#### Pivot Aliases

It is possible to create aliases for the selected paths. This improves the readability and also allows for more succinct config of your segment. It also allows you to work around very long path names that would cause issues with Postgres-flavored databases 63 character limit for column names. Therefore if you are not using aliases your paths can be at most 63 characters long. When using aliases you have to specify aliases for all the paths you want to select. The order of the aliases needs to match the order of the selected paths. The resulting segment's column names will be the ones of your aliases. When using pivot transformations in conjunction with aliases, you need to refer to columns by their aliases in the transformation statements.

An example using aliases could look like this:

Configuration:

```text
env:
- name: PIVOT_PATHS
  value: /customer/info/meta/address, /xyz, /customer/info/meta/contact/phonenumber
- name: PIVOT_ALIASES
  value: street, xyz, phone
- name: PIVOT_TRANSFORMATIONS
  value: street=(? #> arr)->1::text | xyz=? #>> xyz::jsonb | phone=? #>> phone::text
```

Result:

| correlation\_id | street | xyz | phone |
| :--- | :--- | :--- | :--- |
| 1 | ... | ... | ... |
| 2 | ... | ... | ... |
| 3 | ... | ... | ... |

### Flexible Generator

{% hint style="warning" %}
Flexible Generator is available starting from Granary platform version 0.9.1.
{% endhint %}

The flexible generator creates a segment based on a query given by the user. There are **flexible generator** specific variables as specified below.

| Parameter | Description | Default |
| :--- | :--- | :--- |
| `SOURCE_QUERY` | The source query to create the flexible segment from |  |

The flexible source query is internally used to create a table or view with. The SQL issued by the Segment Table Creation looks like `CREATE TABLE AS {SOURCE_QUERY}` or `CREATE VIEW AS {SOURCE_QUERY}`, respectively.

#### Flexible Example

With the flexible generator you are free to create any segment you want. Below you can see a very basic example:

Source table:

| correlation\_id | profile\_type | path | pit | value | ... |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | \_d | /path1 | \_latest | 20 | ... |
| 2 | pt | /path1 | \_latest | 30 | ... |
| 3 | pt | /path1 | \_latest | 40 | ... |
| 4 | pt | /path2 | \_latest | 80 | ... |

Configuration:

```text
env:
- name: SOURCE_QUERY
  value: select count(profile_type), profile_type from profilestore where value < 50 group by profile_type
```

Result:

| count | profile\_type |
| :--- | :--- |
| 1 | \_d |
| 2 | pt |

{% hint style="warning" %}
Please note that if you are using `%` signs in your source query \(e.g. `LIKE '%something'`\), they will need to be prefixed with three more `%` signs to work correctly. The example would therefore look like this: `LIKE '%%%%something'`.
{% endhint %}

## Errors

In case of errors, these are directly thrown and can be detected by the environment running the Docker container \(e.g., Kubernetes\). In case no connection can be established or the database returns an SQL error result, this is the default behaviour of psycopg2. 

The metrics are grouped by the job name set in `PROMETHEUS_JOB`. If the `PROMETHEUS_PUSHGATEWAY` variable is set, the following values are pushed:

* Gauge: `segmentcreation_last_success_unixtime`/`segmentcreation_last_fail_unixtime` for the last success/fail [as recommended via mailing list, can be used to trigger alerts](https://groups.google.com/forum/#!topic/prometheus-developers/rFHMb4vN6cc)
* Summary: `segmentcreation_duration_seconds`, time needed for segmentcreation
* Counter: `segmentcreation_inserts`, total number of inserted rows

