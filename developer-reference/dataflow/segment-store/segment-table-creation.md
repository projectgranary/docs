# Segment Table Creation

## Segment Creation Job

This component generates so-called segments, which can be understood as \(materialized\) views on the data. Depending on the selected storage-layer, segments can be views or tables. The available storage layers are: 

* Citus Data PostgreSQL 
* Amazon Aurora PostgreSQL

Using Citus, segments are distributed tables, co-located with distributed Citus source tables. Targets are populated per shard. Thus, network load is very low and generation takes place in parallel. Using Aurora, segments are configured to be either tables or views \(allowing to leverage Auroras read replicas\).

Currently, two different segment generator types are available:

* Pivot, which generates tuples from a table comprising key/value-like tuples
* Generic, which builds a filtered version of the input table.

## Configure

The creation job runs in a container and can be deployed via helm chart:

`helm install --name <your_name> grnry-stable/segment-table-creation -f <your-values>.yaml --namespace <your-namespace>`

More detailed instructions can be found [here.](../../../operator-reference/installation/segment-table-creation.md) In the _values.yaml_ file configures segment creation parameters. `TYPE` determines the generator type. The following table comprises a complete list of variables \(required if there is no default\).

| Parameter | Description | Default |
| :--- | :--- | :--- |
| `TYPE` | type of generator, `pivot` or `generic` | `pivot` |
| `DB_TYPE` | storage-layer type, `citus` or `aurora` | `citus` |
| `DB_USE_VIEWS` | flag indicating if generated segment should be a view \(aurora-only\) | `false` |
| `DB_HOST` | database endpoint \(citus master host or aurora writer endpoint\) | `grnry-pg-citus-master` |
| `DB_PORT` | database port | `5432` |
| `DB_USER` | postgres user name | Secret Reference needed |
| `DB_PASSWORD` | postgres user password | Secret Reference needed |
| `DB_NAME` | name of the database | Secret Reference needed |
| `DB_USE_SSL` | whether to enforce SSL for DB connection | `true` |
| `SOURCE_SCHEMA_NAME` | name of schema of source table | `public` |
| `SOURCE_TABLE_NAME` | name of source table | `profilestore` |
| `TARGET_SCHEMA_NAME` | name of schema for target segment | `segments` |
| `TARGET_SEGMENT_NAME` | name for target segment, if already existing old segment will be overwritten | `profilestore_seg` |
| `COLUMN_PLACEHOLDER` | placeholder for the path name to be used in the pivot transformation function | ? |
| `TYPE_SEPARATOR` | separator between pivot transformation function and result type | :: |
| `DEFINITION_SEPARATOR` | separator between name and transformation | = |
| `TRANSFORMATION_SEPARATOR` | separator between multiple transformations, e.g., `body_txt=message->'body'::text|body_json=replace(message->'body'#>>'{}', '\', '')::jsonb` | \| |
| `CITUS_DIST_COL` | name of the column the source table is and the target segment will be distributed by. currently, this has to be a single column | correlation\_id |
| `DEBUG` | whether to print all database statement and responses | `true` |
| `SOURCE_WHERE_CLAUSE` | where clause as SQL \(without "WHERE" itself\), must not be empty. |  `"pit='_latest'"` |
| `PROMETHEUS_PUSHGATEWAY` | address of gateway to push prometheus metrics, format `'<host>:<port>'` |  |
| `PROMETHEUS_JOB` |  job name for pushgateway, the metrics will be grouped by this name. |  `segmentcreation` |

### Segment Indexes

There are parameters to define **indexes** on the resulting segment. Note that creating indexes is not possible on segments that are generated as views.

| Parameter | Description | Default |
| :--- | :--- | :--- |
| `TARGET_SEGMENT_INDEX_SEPARATOR` | separator between index definitions | \| |
| `TARGET_SEGMENT_INDEXES` | defines index name and expression. must be in format `<name>=<index expression>.`for example `segidx1=(event_id)` or `segidx2= USING gin (body_json, headers)` |  |

### Segment Views

There are parameters to define **views** on the resulting segment. [See readme](https://gitlab.alvary.io/grnry/segment-table-creation/#views). These views can be used to enforce access regulations. Consider profiles with an `/isLocked` grain set to `true` or `false`  as an example. Then, a segment on such profile can contain that grain, which is further used define two complementary views -- one containing _unlocked_ profiles and a second containing _locked_ profiles. 

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
      <td style="text-align:left">Separator between view <em>name </em>and view <em>condition</em>
      </td>
      <td style="text-align:left"><code>:</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>TARGET_SEGMENT_VIEWS</code>
      </td>
      <td style="text-align:left">
        <p><code>TARGET_SEGMENT_VIEW_SEPARATOR </code>separated list auf views.</p>
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

`<CITUS_DIST_COL>` \(varchar\)

`path` \(text/varchar\)

`value` \(jsonb\)

As an example see the profile store: [https://gitlab.alvary.io/grnry/kafka-profile-update/blob/master/README.md\#L28](https://gitlab.alvary.io/grnry/kafka-profile-update/blob/master/README.md#L28)

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
reader varchar NOT NULL DEFAULT '_auth',
origin varchar,
PRIMARY KEY(id, profile_type, path, pit)
```

For each `<CITUS_DIST_COL>`_/_`path` combination there should be returned a single row whose "`value`"-column is used. Make sure this assumption holds, e.g., via respective where clauses.

#### Pivot Example

Consider the following input table.

| correlation\_id | profile\_type | path | pit | value | certainty | grain\_type | inserted | ttl | reader | origin |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | \_d | a | \_latest | foo | 1 | t | 7 | p1d | \_auth | test |
| 1 | \_d | b | \_latest | bar | 1 | t | 8 | p1d | \_auth | test |
| 1 | \_d | c | \_latest | baz | 1 | t | 9 | p1d | \_auth | test |
| 1 | pt | c | \_latest | bazz | 1 | t | 9 | p1d | \_auth | test |
| 1 | pt | d | \_latest | 3 | 1 | t | 9 | p1d | \_auth | test |
| 2 | pt | c | \_latest | bazz | 1 | t | 9 | p1d | \_auth | test |

Consider the following configuration.

```text
- name: TYPE
  value: "pivot"
- name: CITUS_DIST_COL
  value: "correlation_id"
- name: WHERE_CLAUSE
  value: "profile_type='_d' and pit='_latest' and reader='_auth'"
- name: PIVOT_PATHS
  value: "a,b,c"
```

This results in the following output containing _\_latest_ values for path _a_, _b_ and _c_ per _correlation\_id_.

| correlation\_id | a | b | c |
| :--- | :--- | :--- | :--- |
| 1 | foo | bar | baz |
| 2 | null | null | bazz |

Note that if _PIVOT\_PATHS_ is empty, then it uses all available paths. Determining all path is a potentially long-running operation.

If filtering by paths \(e.g. a in the example above\) the result might contain empty rows:

| correlation\_id | a |
| :--- | :--- |
| 1 | foo |
| 2 | null |

These can be suppressed by setting _ALLOW\_EMPTY_ to _False_. By defining a where clause in _SEGMENT\_FILTER\_CLAUSE_ the output can be further filtered.

####  Transformations of path content

When creating the pivot table you have the option to specify arbitrary transformations on specific paths. These have to be valid sql expressions with a placeholder for the path name. You also have to add the data type of the transformation's result. The resulting column in the pivot table will have this data type.

E.g. `path1=? #>> a::text` will select the element `a` in the jsonb object `path1` as a text column.

The placeholders and separators \(`=`, `?` and `::`\) can be customized if they would otherwise conflict with special characters in the sql expression. Note that all values are initially of type `jsonb`!

Below you can see a full example:

Source table:

| correlation\_id | profile\_type | path | pit | value | ... |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | \_d | a | \_latest | 20 | ... |
| 2 | pt | a | \_latest | 30 | ... |
| 3 | pt | a | \_latest | 60 | ... |

Configuration:

```text
env:
- name: PIVOT_TRANSFORMATIONS
  value: a=(CAST((? #> '{}') AS integer) > 50)::boolean
```

Result:

| correlation\_id | a |
| :--- | :--- |
| 1 | false |
| 2 | false |
| 3 | true |

Common transformations could be:

* Selecting a jsonb path as jsonb object: `path1=? #> a::jsonb`
* Selecting a jsonb path as sql text object: `path1=? #>> a::text`
* Selecting an element from a jsonb array: `path1=(? #> arr)->1::text`
* Converting to a different type: `path1=CAST((? #> '{}') AS DATE)::date` - `#> '{}'` selects the root element of a jsonb object
* Computing a boolean expression: `path1=CAST((? #> '{}') AS int) > 30`

## Errors

In case of errors, these are directly thrown and can be detected by the environment running the Docker container \(e.g., Kubernetes\). In case no connection can be established or the Citus master returns an SQL error result, this is the default behaviour of psycopg2. In case executing a command on the workers fails, the master will not return an error. The segmentcreation tool detects this by checking the second last return value of `run_command_on_workers` or `run_command_on_colocated_placements` indicating `success`, according to [psql's `df+` output](https://www.postgresql.org/docs/current/app-psql.html):

*  `OUT nodename text, OUT nodeport integer, OUT success boolean, OUT result text` for `run_command_on_workers`
*  `OUT nodename text, OUT nodeport integer, OUT shardid1 bigint, OUT shardid2 bigint, OUT success boolean, OUT result text` for `run_command_on_colocated_placements`

However, this interface is not documented and thus might change in future versions of Citus.

The metrics are grouped by the job name set in `PROMETHEUS_JOB`. If the `PROMETHEUS_PUSHGATEWAY` variable is set, the following values are pushed:

* Gauge: `segmentcreation_last_success_unixtime`/`segmentcreation_last_fail_unixtime` for the last success/fail [as recommended via mailing list, can be used to trigger alerts](https://groups.google.com/forum/#!topic/prometheus-developers/rFHMb4vN6cc)
* Summary: `segmentcreation_duration_seconds`, time needed for segmentcreation
* Counter: `segmentcreation_inserts`, total number of inserted rows



