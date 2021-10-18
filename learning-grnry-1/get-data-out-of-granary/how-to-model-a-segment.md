---
description: >-
  This page helps you understand how to model a segment and what decisions to
  make during the design process.
---

# How to model a Segment

In the following we want to describe from start to finish how to model and create segments in Granary. Check out the [Segment Store](../../developer-reference/dataflow/segment-store/) and its subpages to learn what a segment is conceptionally. 

## 1. Decide which data to use

First you need to decide which data to create the segment on. If you want to create your segment on the data saved in key-value profiles you will use the [Profile Store](../../developer-reference/dataflow/profile-store/). If you want to work with event data you will use the [Event Store](../../developer-reference/dataflow/event-store/).

In our example, we want to create a segment on the Event Store so in the configuration of our segment`SOURCE_TABLE_NAME` will be set to `eventstore`.

## 2. Decide which segment type to use

As described in the [Segment Table Creation](../../developer-reference/dataflow/segment-store/segment-table-creation.md) page, there are different types (`pivot`, `generic`,  `flexible`). \
When creating a segment on the Profile Store data, we recommend a pivot segment, as the data in the Profile Store is stored as flattened key-value pairs. Thus, to be able to easily access certain paths in a segment you need to pivot them to select them as regular DB columns.\
The generic segment creates a projection of the specified data and also allows tranforming the raw data.\
The flexible segment takes an SQL user query as input and is therefore able to freely create a segment as you like.\
\
In our example, we want to create a transformed projection of the data in the Event Store, so in our segment configuration  `TYPE`  will be set to `generic`.

{% hint style="warning" %}
Please note that due to technical limitations, `pivot`segments might fail if many grains (\~5.000.000) are being selected and may even cause database restarts.
{% endhint %}

## 3. Select your input data

If you want to create your segment just on a subset of the source data, such as only events written by a certain [Harvester](../data-in/how-to-run-a-harvester/harvesters.md), you can do so by specifying a `SOURCE_WHERE_CLAUSE` in your segment config. \
If instead you want to use all available source data you do not need to include an extra parameter. 

In our example, we want to only project events written by the 'adobe' harvester, thus we set `SOURCE_WHERE_CLAUSE` to `event_harvester = 'adobe'`.

## 4. Define your segment specifics

Next you want to define the specific configuration for the segment type you chose. Please consult the [Segment Table Creation Documentation ](../../developer-reference/dataflow/segment-store/segment-table-creation.md)to see which parameters to set depending on the segment type.

As we want to create a generic segment, we will have to specify which columns to project (`GENERIC_COLUMNS`) and any additional transformation we want to do (`GENERIC_TRANSFORMATIONS`).  We want to select the `event_id `and `event_harvester` columns from the Event Store, so we set `GENERIC_COLUMNS `to `event_id,event_harvester`.  Furthermore we want to select parts of the `message `column. In our example we know the message contains among other things a value `struc` and a description `desc` which we want to transform. The `message` column of the Event Store is of type jsonb, so we will work with [JSON operators](https://www.postgresql.org/docs/current/functions-json.html). For `struc` we just want to include it as a column `structure `of type jsonb. For `desc` we want to include as a column `description `of type text that is all upper-cased.  Our `GENERIC_TRANSFORMATIONS` parameter will therefore be set to `structure=message->'struc'::jsonb|description=upper(message->'desc'#>>'{}')::text`.

{% hint style="info" %}
Please note that the Event Stores `message` and the Profile Stores `value` column are of type `jsonb`. To efficiently work with segments using these columns typecasting them is often necessary .
{% endhint %}

## 5. Select your segment persistence  

Decide if your segment should be stored in the database as a table or as a view. \
If you opt for table the segment will be created from the data available at the start of the segment creation. You can quickly query the data and you can create indexes to further improve performance. \
If you opt for view the segment will be stored as a query in the database and will be run each time you access the view, thus always having the most recent data available at the cost of performance. \
If you opt for table you do not need to include an extra parameter, if you opt for view you will need to set `DB_USE_VIEWS `to `True`.

In our example, we are going to create a table segment, so we do not specify any extra parameter for the persistence.

## 6. Decide if you want to create indexes or views

Decide if you want to create indexes or views on top of your segment. \
If you want to create indexes you need to specify the index definition(s) in the `TARGET_SEGMENT_INDEXES` parameter. Please note that creating indexes is only possible on table segments.\
If you want to create views you need to specify the view definition(s) in the `TARGET_SEGMENT_VIEWS` parameter.

In this case we want to create an index `idx1` on the columns `event_id `and `description`. Thus we include the parameter `TARGET_SEGMENT_INDEXES` set to `idx1=(event_id, description)` in our config.

{% hint style="info" %}
Creating indexes can be especially beneficial for range-based queries like time intervals.
{% endhint %}

## 7. Define your segment name

Define a name for your segment by setting the `TARGET_SEGMENT_NAME` parameter. See [hints on naming of Segments](best-practices/hints-on-naming-of-segments.md) for further information.

We will set `TARGET_SEGMENT_NAME` to `adobe_events_info`.

## 8. Define your database specifics

Define the database-specific parameters of the segment. \
If you are using PostgreSQL (or any PostgreSQL flavor like Amazon Aurora), set `DB_TYPE` to `postgres`.  If you are using Citus, set `DB_TYPE` to `citus` and the `CITUS_DIST_COL` parameter which defines on which column Citus would distribute its table. You usually want to set this to `correlation_id`.\
If you are still in the development phase of your segment it is also a good idea to set `DEBUG` to `True` so you can later take a look at the logs of the segment creation in case something went wrong.

In our example, we are going to set `DB_TYPE` to `postgres `and `DEBUG` to `True`.

## 9. Configure the segment CRD

Now that we modeled our segment it is time to configure our segment CRD. For this you will need to set the `displayName` attribute and the [cron](https://crontab.guru) `schedule` for the creation job. \


We will set our `displayName` to `Adobe Event Info Segment`, similar to its `TARGET_SEGMENT_NAME`. We chose to run our job every full hour, so we set its [cron](https://crontab.guru) `schedule` to `0 * * * *`.

## 10. Send your segment to the API

The complete request we are going to send to the [Segment Management API](broken-reference) thus looks like this:

```bash
 curl -X POST -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @segment.json \
 https://grnry-host/segments
```

{% code title="segment.json" %}
```javascript
{
    "metadata": {
        "displayName": "Adobe Event Info Segment"
    },
    "data": {
        "cronjob": {
            "schedule": "0 * * * *"
        },
        "env": {
            "DB_TYPE": "postgres",
            "DEBUG": "True",
            "GENERIC_COLUMNS": "event_id,event_harvester",
            "GENERIC_TRANSFORMATIONS": "structure=message->'struc'::jsonb|description=upper(message->'desc'#>>'{}')::text",
            "SOURCE_TABLE_NAME": "eventstore",
            "SOURCE_WHERE_CLAUSE": "event_harvester = 'adobe'",
            "TARGET_SEGMENT_INDEXES": "idx1=(event_id, description)",
            "TARGET_SEGMENT_NAME": "adobe_events_info",
            "TYPE": "generic"
        }
    }
}
```
{% endcode %}

