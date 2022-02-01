---
description: Reference page how data segmentation is done in Granary with DBT
---

# dbt Segment

## Introduction

Granary uses [dbt](https://www.getdbt.com) Framework to define segments and the [Belt API](../../api-reference/belt-api.md#create-and-store-a-belt) to execute their creation.

dbt performs data transformation using a `select` statements which converts Profile or Raw Event data into segments. Depending upon the configuration, the segment can be accessed as Table/View against Event Store or Profile Store.&#x20;

Each segment will be created in the project's database schema. The user is not responsible to define or configure any DDL statements. Table/Views will be created by DBT Framework. So no extra configuration required. The dbt segment creation is executed with the [project's database user](../../projects.md#docs-internal-guid-06a5e216-7fff-927b-e181-d2437d40037b).

The users can access Views/Tables inside that schema using Granary's built-in SQL web console [SQLPad](https://getsqlpad.com). Users who don't have permission to access that project cannot access the segment.

The available storage layers are **PostgreSQL** (AWS Aurora / Azure Postgres) that is configured for Granary.

{% hint style="info" %}
For practical example, see [How to model a Segment with a DBT Belt](../../../learning-grnry-1/get-data-out-of-granary/how-to-model-a-segment-with-a-dbt-belt.md).
{% endhint %}

## Configure Segment Definition

As stated before, dbt Segment are executed via the Belt API. Belt models carrying the property `"beltType": "dbt-segment"` are considered by the API as such.

Furthermore, two more mandatory properties for DBT Belt exist:

* `"segmentDefinition"` **** --> string-escaped SQL select statement
* `"cronExpression"`

### **Segment Definition**&#x20;

User can Configure Segment definition using dbt SQL syntax.

Previously in the [Segment Table Creation](segment-table-creation.md), three different segment generator types where available. Those types do not exist any longer. All of them can be expressed with plain SQL in dbt.

#### **Pivot Segment**

`select` statement used to fetch the data from Profile Store. PostgreSQL's [crosstab function](https://www.postgresql.org/docs/12/tablefunc.html) is used to pivot data from Profile Store's key-value data format to a row-based format which is desirable in a segment. To do so, users have to provide the profile's grain `path`s to fetch from the grain's `value` column. In the crosstab function, after the VALUES keyword, the list of grain paths to include in the pivot have to be provided. If the grain path is greater than 63 character, MD5 hash of the `path` has to be given.

```sql
SELECT
  correlation_id,
  "/test1021" AS "/test1021",
  "/test1051" AS "/test1051"
FROM
  crosstab(
    'SELECT  correlation_id,  MD5(path), value FROM "project-name"."profilestore_view" WHERE pit=''_latest'' ORDER  BY 1,2',
    $ $
    VALUES
      ('a29baeaf8d0eae94222a689560793fa7'), ('a20b90a84fec6d7ee4897498b0eb6efa') $$
  ) 
  AS ct ("correlation_id" varchar, "/test1021" jsonb, "/test1051" jsonb)
  WHERE "/test1021" IS NOT NULL;
```

Where the MD5 hashes in line 10 are the hashed grain paths, e.g.:

```bash
$ printf '/test1021' | md5sum
a29baeaf8d0eae94222a689560793fa7
```

****

**Generic Segment**

Using Generic Segment**,** users can traverse the JSON value in Event Store's `message` column to fetch and transform the appropriate data.

```sql
SELECT
  created,
  "correlation_id",
  (message -> 'data' -> 0 -> 'ue_pr' -> 'data' -> 'data' -> 'body' -> 'Route' #>>'{}')::route,
  (message->'data'->0->'ue_pr'->'data'->'data'->'body'->'Result'#>>'{}')::result,
  (message->'data'->0->'ue_pr'->'data'->'data'->'body'->'Timestamp'#>>'{}')::timestamp,
  (message->'data'->0->'ue_pr'->'data'->'data'->'environment'->'cid'#>>'{}')::cid,
  (message->'data'->0->'ue_pr'->'data'->'data'->'event'->'source_type'#>>'{}')::sourceType
   FROM "project-name"."eventstore_view";
```

****

#### **Flexible Segment**

User can write their own `select` query to fetch data from Profile/Event store **** or any other table accesible for the project's database user. Config configuration can be given to define either the schema will be Table or View in the Database. You can also define indexes over the column to make it work faster. Below mentioned example demonstrate the same.

```sql


#From Profile Store
select 'from_profile' as new_column_from_profiletype, * from global."profilestore_dbt-belt-test"


#From Event Store
select 'from_event' as new_column_from_eventtype, * from global."eventstore_snowplow-a"

```



#### Materialization mode

dbt allows to provide configuration within the SQL file. See [Materializations](https://docs.getdbt.com/docs/building-a-dbt-project/building-models/materializations).

```
// Some #Config to create the View/Table
{{ 
	config(
    	materialized='View/Table'
	) 
}}


SQL SELECT goes here
```

#### Index

```
#Config to create Indexes out of the Column
{{ 
	config(
    	materialized='table',
    	indexes=[
      		{'columns': ['new_column_from_eventtype_cc'], 'type': 'hash'}
    	]
	) 
}}

SQL SELECT goes here
```



### **Cron Expression**

Use CRON syntax to specify when you'd like your job to run. CRON syntax is very expressive, and allows you to completely customize your run schedule.

```
// Examples
0 * * * *: Every hour, at minute 0
*/5 * * * *: Every 5 minutes
5 4 * * *: At exactly 4:05 AM UTC
30 */4 * * *: At minute 30 past every 4th hour (e.g. 4:30AM, 8:30AM, 12:30PM, etc., all UTC)
0 0 */2 * *: At midnight UTC every other day
0 0 * * 1: At midnight UTC every Monday.
```

To use cron expression place your syntax in the belt definition as described below.

```json
{
	"name": "dbt-belt-cc",
	"description": "segment belt - custom config ins sql",
	"eventTypes": [
		"snowplow-a"
	],
	"beltType": "dbt-segment",
	"outputTypes": [
		{
			"name": "dbt-belt-cc",
			"version": "latest"
		}
	],
	"segmentDefinition": "...",
	"cronExpression": "*/1 * * * *"
}
```
