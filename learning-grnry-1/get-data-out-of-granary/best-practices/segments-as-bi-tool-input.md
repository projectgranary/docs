# Segments as BI Tool Input

Profiles contain integrated views on your entities. They comprise master data and pre-defined aggregates. If profiles contain \(copies of\) events, then these are a special form of very fine-grained aggregates, which we do not recommend. It is good practice to define what you need  for your use case and store solely what is required. If you cannot integrate or aggregate data, then the entity you profile or the way you model your entity might not be the right choice.

In the following, we quickly demonstrate how to use aggregates, namely counts, stored in profiles to create input, which nicely fits into BI tools. Please note that this is somewhat  technical and you must know a few Profile Store internals and be familiar with Segments.

Consider the following two \(simplified\) profiles including aggregates per month as an example. These have a fragment `visits`, which contain data for `2019-07` to `2019-09`. Clearly, such grains of the form `YYYY-MM` can be created in belts.

```text
{
  "_id": "007",
  "visits": {
    "_schema": "visits",
    "2018-09": {
      "_latest": {
        "_v": 30,
        "_in": "2018-09-30"
      }
    },
    "2018-08": {
      "_latest": {
        "_v": 23,
        "_in": "2018-08-31"
      }
    },
    "2018-07": {
      "_latest": {
        "_v": 26,
        "_in": "2018-08-31"
      }
    }
  }
}
{
  "_id": "008",
  "visits": {
    "_schema": "visits",
    "2018-09": {
      "_latest": {
        "_v": 31
      }
    },
    "2018-08": {
      "_latest": {
        "_v": 24
      }
    },
    "2018-07": {
      "_latest": {
        "_v": 27
      }
    }
  }
}
```

We now turn this into the following long table which is a good input for BI tools.

| profile | path | value |
| :--- | :--- | :--- |
| 007 | /visits/2018-09 | 30 |
| 007 | /visits/2018-08 | 23 |
| 007 | /visits/2018-07 | 26 |
| 008 | /visits/2018-09 | 31 |
| 008 | /visits/2018-08 | 24 |
| 008 | /visits/2018-07 | 27 |

The profile store captures the data as key/value pairs.

| correlation\_id | profile\_type | path | pit | value | grain\_type |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 007 | agency | /visits/2018-07 | \_latest | {"\_step": 1, "\_current": 26, "\_initial": 0} | c |
| 007 | agency | /visits/2018-08 | \_latest | {"\_step": 1, "\_current": 23, "\_initial": 0} | c |
| 007 | agency | /visits/2018-09 | \_latest | {"\_step": 1, "\_current": 30, "\_initial": 0} | c |
| 008 | agency | /visits/2018-07 | \_latest | {"\_step": 1, "\_current": 27, "\_initial": 0} | c |
| 008 | agency | /visits/2018-08 | \_latest | {"\_step": 1, "\_current": 24, "\_initial": 0} | c |
| 008 | agency | /visits/2018-09 | \_latest | {"\_step": 1, "\_current": 31, "\_initial": 0} | c |

From this, the following query creates a long result table comprising monthly aggregates from the _last year_. Clearly, arbitrary path patterns in the where clause are possible. In this example, we look for the _visits_ fragment and determine the _last year_.

```sql
select -- our output schema
  correlation_id,
  profile_type,
  substring(path from 9 for 7),
  value->'_current'#>>'{}' -- grab the current value from the counter
from profilestore
-- find path like '/visits/2018*' assuming we are in 2019
where path like '/visits/' || date_part('year', CURRENT_DATE - interval '1 year')::integer || '%'
-- uses the latest grain value
and pit = '_latest'
-- select only counters
and grain_type = 'c'
```

From here, we can write a segment configuration. Note that we apply `generic segments` on the profile store \(which has key/values under the hood\). We use a  `SOURCE_WHERE_CLAUSE` to select the grains comprising our aggregates - namely visits from last year. Finally, `GENERIC_TRANSFORMATIONS` let us grab the current value from the counter in the store.

```text
env:
  ###############
  SOURCE_SCHEMA_NAME: "public"
  TARGET_SCHEMA_NAME: "segments"

  SOURCE_TABLE_NAME: "profilestore"
  TARGET_SEGMENT_NAME: "profilestore_visits"

  ###############
  SOURCE_WHERE_CLAUSE: "path like '/visits/' || date_part('year', CURRENT_DATE - interval '1 year')::integer || '%%%%'
                        and pit = '_latest'
                        and grain_type = 'c'" 

  ###############
  TYPE: "generic"
  GENERIC_COLUMNS: "profile_type"
  GENERIC_TRANSFORMATIONS: "path=substring(path from 9 for 7)::text|current=value->'_current'#>>'{}'::text"
```

\(We will work on making this a more user friendly flow.\)

