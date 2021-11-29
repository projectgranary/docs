---
description: This page specifies the configuration of segments in Granary.
---

# Segment Table Creation

## Segment Creation Job

This component generates so-called segments, which can be understood as relational view on Profile Store or Event Store data.&#x20;

Depending on the segment's access pattern, segments can be **views** or **tables:**

* **View segments** are always up-to-date as queries are run at access time against Profile Store / Event Store.
* **Table segments** are updated based on a cron schedule, are pre-computed, and can be equipped with additional indexes. During the scheduled pre-computation, the old segment is still fully accessible.

The available storage layers are:

* **PostgreSQL** (compatible with Amazon Aurora - allowing to leverage read replicas for view segments)
* **Citus** Data PostgreSQL

Using standard **PostgreSQL**, table segments are normal database tables. Using **Citus**, table segments are distributed tables, co-located with distributed Citus source tables. Targets are populated per shard. Thus, network load is very low and generation takes place in parallel.&#x20;

Currently, three different segment generator types are available:

* **Pivot**, which generates tuples from a table comprising key/value-like tuples
* **Generic**, which builds a filtered version of the input table
* **Flexible**, which takes a user-given query to create its segment

{% hint style="warning" %}
Please note that due to technical limitations, `pivot`segments might fail if many grains (\~5.000.000) are being selected and may even cause database restarts.
{% endhint %}

## Configure

The creation job runs in a container and can be deployed via [Segment Management API](broken-reference). The following table comprises a complete list of variables (required if there is no default):

| Parameter                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Default                 |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| `TYPE`                     | type of generator, `pivot` , `generic` or `flexible`                                                                                                                                                                                                                                                                                                                                                                                                              | `pivot`                 |
| `DB_TYPE`                  | storage-layer type, `citus` or `postgres`                                                                                                                                                                                                                                                                                                                                                                                                                         | `postgres`              |
| `DB_USE_VIEWS`             | flag indicating if generated segment should be a view                                                                                                                                                                                                                                                                                                                                                                                                             | `false`                 |
| `DB_PRESERVE_VIEWS`        | <p>Segments are recreated based on a CRON schedule. If a custom-made view relies on a Segment, it is dropped and needs to be recreated after update. With this flag set to <code>true</code>, manually created views on the Segment are preserved.</p><p>WARNING: With this feature Segment columns can only be added and not be changed. On breaking change of the segment, just run the segment job with this flag disabled and enable it again afterwards.</p> | `false`                 |
| `DB_HOST`                  | database endpoint (citus master host or aurora writer endpoint)                                                                                                                                                                                                                                                                                                                                                                                                   | `grnry-pg`              |
| `DB_PORT`                  | database port                                                                                                                                                                                                                                                                                                                                                                                                                                                     | `5432`                  |
| `DB_USER`                  | database user name                                                                                                                                                                                                                                                                                                                                                                                                                                                | Secret Reference needed |
| `DB_PASSWORD`              | database user password                                                                                                                                                                                                                                                                                                                                                                                                                                            | Secret Reference needed |
| `DB_NAME`                  | name of the database                                                                                                                                                                                                                                                                                                                                                                                                                                              | Secret Reference needed |
| `DB_USE_SSL`               | whether to enforce SSL for DB connection                                                                                                                                                                                                                                                                                                                                                                                                                          | `true`                  |
| `SOURCE_SCHEMA_NAME`       | name of schema of source table                                                                                                                                                                                                                                                                                                                                                                                                                                    | `public`                |
| `SOURCE_TABLE_NAME`        | name of source table                                                                                                                                                                                                                                                                                                                                                                                                                                              | `profilestore`          |
| `TARGET_SCHEMA_NAME`       | name of schema for target segment                                                                                                                                                                                                                                                                                                                                                                                                                                 | `segments`              |
| `TARGET_SEGMENT_NAME`      | name for target segment, if already existing old segment will be overwritten                                                                                                                                                                                                                                                                                                                                                                                      | `profilestore_seg`      |
| `COLUMN_PLACEHOLDER`       | placeholder for the path name to be used in the pivot transformation function                                                                                                                                                                                                                                                                                                                                                                                     | `?`                     |
| `TYPE_SEPARATOR`           | separator between pivot transformation function and result type                                                                                                                                                                                                                                                                                                                                                                                                   | `::`                    |
| `DEFINITION_SEPARATOR`     | separator between name and transformation                                                                                                                                                                                                                                                                                                                                                                                                                         | `=`                     |
| `TRANSFORMATION_SEPARATOR` | separator between multiple transformations, e.g. `body_txt=message->'body'::text\|body_json=replace(message->'body'#>>'{}', '\', '')::jsonb`                                                                                                                                                                                                                                                                                                                      | `\|`                    |
| `CITUS_DIST_COL`           | Name of the column the source table is and the target segment will be distributed by. Currently, this has to be a single column and is only mandatory if `DB_TYPE=citus`.                                                                                                                                                                                                                                                                                         | correlation\_id         |
| `DEBUG`                    | whether to print all database statement and responses                                                                                                                                                                                                                                                                                                                                                                                                             | `true`                  |
| `SOURCE_WHERE_CLAUSE`      | where clause as SQL (without "WHERE" itself), must not be empty.                                                                                                                                                                                                                                                                                                                                                                                                  | `"pit='_latest'"`       |
| `PROMETHEUS_PUSHGATEWAY`   | address of gateway to push prometheus metrics, format `'<host>:<port>'`                                                                                                                                                                                                                                                                                                                                                                                           |                         |
| `PROMETHEUS_JOB`           | job name for pushgateway, the metrics will be grouped by this name.                                                                                                                                                                                                                                                                                                                                                                                               | `segmentcreation`       |

### Segment Indexes

There are parameters to define **indexes** on the resulting segment. Note that creating indexes is not possible on segments that are generated as views.

| Parameter                        | Description                                                                                                                                                                                                               | Default |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `TARGET_SEGMENT_INDEX_SEPARATOR` | Separator between index definitions                                                                                                                                                                                       | \|      |
| `TARGET_SEGMENT_INDEXES`         | <p>Defines index name and expression. Must be in format <code>&#x3C;name>=&#x3C;index expression></code>. <br>For example:<br><code>segidx1=(event_id)</code> or <code>segidx2= USING gin (body_json, headers)</code></p> |         |

### Segment Views

There are parameters to define **views** on the resulting segment. These views can be used to enforce access regulations. Consider profiles with an `/isLocked` grain set to `true` or `false` as an example. Then, a segment on such profile can contain that grain, which is further used define two complementary views -- one containing _unlocked_ profiles and a second containing _locked_ profiles.

For this example, the respective configuration could look as follows:

`PIVOT_PATHS: "/pathA,/pathB,/pathC,/isLocked"`  \
`PIVOT_TRANSFORMATIONS: "/isLocked= ?#>>'{}'::text"`  \
`TARGET_SEGMENT_VIEWS: "locked:\"/isLocked\" = 'true',unlocked:\"/isLocked\" = 'false'"`

Detailed configuration

| Parameter                                  | Description                                                                                                                                          | Default |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `TARGET_SEGMENT_VIEW_SEPARATOR`            | Separator between view definitions                                                                                                                   | `,`     |
| `TARGET_SEGMENT_VIEW_DEFINITION_SEPARATOR` | Separator between view _name_ and view _condition_                                                                                                   | `:`     |
| `TARGET_SEGMENT_VIEWS`                     | <p><code>TARGET_SEGMENT_VIEW_SEPARATOR</code> separated list auf views.</p><p>Definition of a view: <em>name definition-separator condition</em></p> | -       |

One can now set access to the resulting views, containing either unlocked or locked profiles, via Granary's [IAM control](../../../operator-reference/identity-and-access-management/).

### Generic Generator

The generic generator creates a filtered projection from the source table. Data is selected using the specified where clause.

Also, there are **generic generator** specific variables as specified below.

| Parameter                 | Description                                                                                                                       | Default                                                                                                                                                                                                                                            |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `GENERIC_COLUMNS`         | source columns to be used without transformation                                                                                  | event\_id                                                                                                                                                                                                                                          |
| `GENERIC_TRANSFORMATIONS` | columns to be created from transformation. must be in format `<name><definition_separator><sql expression><type separator><type>` | <p><code>body_txt=</code></p><p><code>message->'body'::text|</code></p><p><code>body_json=</code></p><p><code>replace(message->'body'#>>'{}', '\', '')::jsonb|</code></p><p><code>headers=</code></p><p><code>message->'headers'::jsonb</code></p> |

### Pivot Generator

Additionally, there are **pivot generator** specific variables as specified below.

| Parameter                    | Description                                                                                                                                                                                                    | Default |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| `PIVOT_PATHS`                | list of paths to be turned into columns, comma-separated, no explicit spec means all paths                                                                                                                     |         |
| `PIVOT_ALIASES`              | list of aliases for the paths to be turned into columns, comma-separated                                                                                                                                       |         |
| `PIVOT_TRANSFORMATIONS`      | <p>list of transformations to apply on paths;</p><p>must be in the format <code>&#x3C;path>&#x3C;definition_separator>&#x3C;sql expression with placeholder>&#x3C;type separator>&#x3C;result type></code></p> |         |
| `PIVOT_ALLOW_EMPTY`          | whether to remove empty rows from the segment output                                                                                                                                                           | `true`  |
| `PIVOT_SEGMENT_WHERE_CLAUSE` | where clause as SQL (without `WHERE` itself) to filter segments after segmentation, e.g., `a::text = '\"foo\"'` (note that column values are of type `jsonb`)                                                  |         |

The pivot generator presupposes that the following input columns are present in the source table.

`<CITUS_DIST_COL>` (varchar, only necessary if `DB_TYPE=citus`)

`path` (text/varchar)

`value` (jsonb)

As an example see the ProfileStore:

```
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

| correlation\_id | profile\_type | path   | pit      | value | certainty | grain\_type | inserted | ttl                 | ttn | reader | origin |
| --------------- | ------------- | ------ | -------- | ----- | --------- | ----------- | -------- | ------------------- | --- | ------ | ------ |
| 1               | \_d           | /path1 | \_latest | foo   | 1         | t           | 7        | <p></p><p>p100y</p> | p1d | \_auth | test   |
| 1               | \_d           | /path2 | \_latest | bar   | 1         | t           | 8        | <p></p><p>p100y</p> | p1d | \_auth | test   |
| 1               | \_d           | /path3 | \_latest | baz   | 1         | t           | 9        | <p></p><p>p100y</p> | p1d | \_auth | test   |
| 1               | pt            | /path3 | \_latest | bazz  | 1         | t           | 9        | <p></p><p>p100y</p> | p1d | \_auth | test   |
| 1               | pt            | /path4 | \_latest | 3     | 1         | t           | 9        | <p></p><p>p100y</p> | p1d | \_auth | test   |
| 2               | pt            | /path3 | \_latest | bazz  | 1         | t           | 9        | <p></p><p>p100y</p> | p1d | \_auth | test   |

Consider the following configuration.

```
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
| --------------- | ------ | ------ | ------ |
| 1               | foo    | bar    | baz    |
| 2               | null   | null   | bazz   |

Note that if _PIVOT\_PATHS_ is empty, then it uses all available paths. Determining all path is a potentially long-running operation.

If filtering by paths (e.g. /path1 in the example above) the result might contain empty rows:

| correlation\_id | /path1 |
| --------------- | ------ |
| 1               | foo    |
| 2               | null   |

These can be suppressed by setting _ALLOW\_EMPTY_ to _False_. By defining a where clause in _SEGMENT\_FILTER\_CLAUSE_ the output can be further filtered.

#### Transformations of path content

When creating the pivot table you have the option to specify arbitrary transformations on specific paths. These have to be valid sql expressions with a placeholder for the path name. You also have to add the data type of the transformation's result. The resulting column in the pivot table will have this data type.

E.g. `/path1=? #>> a::text` will select the element `a` in the jsonb object of `/path1` as a text column.

The placeholders and separators (`=`, `?` and `::`) can be customized if they would otherwise conflict with special characters in the sql expression. Note that all values are initially of type `jsonb`!

Below you can see a full example:

Source table:

| correlation\_id | profile\_type | path   | pit      | value | ... |
| --------------- | ------------- | ------ | -------- | ----- | --- |
| 1               | \_d           | /path1 | \_latest | 20    | ... |
| 2               | pt            | /path1 | \_latest | 30    | ... |
| 3               | pt            | /path1 | \_latest | 60    | ... |

Configuration:

```
env:
- name: PIVOT_TRANSFORMATIONS
  value: /path1=(CAST((? #> '{}') AS integer) > 50)::boolean
```

Result:

| correlation\_id | /path1 |
| --------------- | ------ |
| 1               | false  |
| 2               | false  |
| 3               | true   |

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

```
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
| --------------- | ------ | --- | ----- |
| 1               | ...    | ... | ...   |
| 2               | ...    | ... | ...   |
| 3               | ...    | ... | ...   |

### Flexible Generator

{% hint style="warning" %}
Flexible Generator is available starting from Granary platform version 0.9.1.
{% endhint %}

The flexible generator creates a segment based on a query given by the user. There are **flexible generator** specific variables as specified below.

| Parameter      | Description                                          | Default |
| -------------- | ---------------------------------------------------- | ------- |
| `SOURCE_QUERY` | The source query to create the flexible segment from |         |

The flexible source query is internally used to create a table or view with. The SQL issued by the Segment Table Creation looks like `CREATE TABLE AS {SOURCE_QUERY}` or `CREATE VIEW AS {SOURCE_QUERY}`, respectively.

#### Flexible Example

With the flexible generator you are free to create any segment you want. Below you can see a very basic example:

Source table:

| correlation\_id | profile\_type | path   | pit      | value | ... |
| --------------- | ------------- | ------ | -------- | ----- | --- |
| 1               | \_d           | /path1 | \_latest | 20    | ... |
| 2               | pt            | /path1 | \_latest | 30    | ... |
| 3               | pt            | /path1 | \_latest | 40    | ... |
| 4               | pt            | /path2 | \_latest | 80    | ... |

Configuration:

```
env:
- name: SOURCE_QUERY
  value: select count(profile_type), profile_type from profilestore where value < 50 group by profile_type
```

Result:

| count | profile\_type |
| ----- | ------------- |
| 1     | \_d           |
| 2     | pt            |

{% hint style="warning" %}
Please note that if you are using `%` signs in your source query (e.g. `LIKE '%something'`), they will need to be prefixed with three more `%` signs to work correctly. The example would therefore look like this: `LIKE '%%%%something'`.
{% endhint %}

## Errors

In case of errors, these are directly thrown and can be detected by the environment running the Docker container (e.g., Kubernetes). In case no connection can be established or the database returns an SQL error result, this is the default behaviour of psycopg2.&#x20;

The metrics are grouped by the job name set in `PROMETHEUS_JOB`. If the `PROMETHEUS_PUSHGATEWAY` variable is set, the following values are pushed:

* Gauge: `segmentcreation_last_success_unixtime`/`segmentcreation_last_fail_unixtime` for the last success/fail [as recommended via mailing list, can be used to trigger alerts](https://groups.google.com/forum/#!topic/prometheus-developers/rFHMb4vN6cc)
* Summary: `segmentcreation_duration_seconds`, time needed for segmentcreation
* Counter: `segmentcreation_inserts`, total number of inserted rows
