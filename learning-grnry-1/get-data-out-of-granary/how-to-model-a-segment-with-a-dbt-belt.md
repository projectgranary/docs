---
description: >-
  This page is to explain about the creation of DBT Belts that define Segments
  using SQL.
---

# How to model a Segment with a DBT Belt

### Step 1 - Create Project (optional)

_Only carry out this step if you do not have a Granary project yet. For details refer to the_ [_Create Project in Granary_](../create-project-in-granary.md) _guide._



### Step 2 - Create Input Event Type

DBT Belts require input Event Types that reference a database location. Currently, Granary supports the following Event Types as input for DBT Belts:

* Data-In --> database location is a view on [Event Store](../../developer-reference/dataflow/event-store/)
* Profile --> database location is a view on [Profile Store](../../developer-reference/dataflow/profile-store/)
* Live-Segment --> database location is the reference Live-Segment table
* Segment --> database location is the output table of another DBT Belt

To create the input Event Type, we either use Granary UI or the Harvester API with the following request:

[POST /projects/:project-name/event-types](../../developer-reference/api-reference/harvester-api/event-type-endpoints/#create-an-event-type)

where `:project-name` needs to be provided as input to the request (we can refer it from step 1).

```json
{
    "displayName": "dbt-belt-inputET-projectfortest",
    "description": "dbt belt for projectfortest",
    "type": "data_in",
    "eventIdExpression": "#randomUUID()",
    "timestampExpression": "#nowMillis()",
    "correlationIdExpression": "#randomUUID()",
    "eventstoreTTL": "P100Y"
}
```

The Data-In Event Type looks like this in Granary UI:

![](<../../.gitbook/assets/Screenshot 2021-10-21 at 17.40.43.png>)

Please notice the referenced database location and the SQLPad link in the context menu in the upper right corner.&#x20;

{% hint style="info" %}
In case of Data-In and Live-Segment, please ensure that their persisters (right node in the graph) have the state running. Otherwise, no data will be written to the datebase.
{% endhint %}



### Step 3 - Create Output EventType

DBT Belts require exactly one output Event Type. Therefore, we need to create a Segment Event Type whose name determines the datbase table's name using Granary UI or by calling:

[POST /projects/:project-name/event-types](../../developer-reference/api-reference/harvester-api/event-type-endpoints/#create-an-event-type)

&#x20;where `:project-name` needs to be provided as input to the request (we can refer it from step 1).

```json
{
    "displayName": "dbt-belt-outputET-projectfortest",
    "type": "segment"
}
```

The Segment Event Type looks like this in Granary UI:&#x20;

![](<../../.gitbook/assets/Screenshot 2021-10-21 at 17.47.02.png>)



### Step 4 - Create Harvester&#x20;

To be able to model a segment in SQLPad, we need to ensure that there is a Harvester serving our input event type(s) with data. Follow the [Harvester creation guide](../data-in/how-to-run-a-harvester/harvesters.md) and reference the input Event Type created in step 2. Do not forget to start the Harvester after creation:

![](<../../.gitbook/assets/Screenshot 2021-10-21 at 18.01.33.png>)

After that, check the Event Type from step 2 in Granary UI again and see the Harvester depicted as input node:

![](<../../.gitbook/assets/image (75).png>)



### Step 5 - Send & access Data&#x20;

Send sample data to your Harvester and try to access it in SQLPad using the view on Event Store from the input Event Type (in case of a Data-In Event Type).

Open the SQLPad UI from your project via the context menu of the Event Type in Granary UI. In SQLPad, we can see the schema created for your project using the `:project-name` that we specified in step 1. From the editor in the center of the screen, we can access the tables and views in our project's the schema.

![ ](<../../.gitbook/assets/Screenshot 2021-10-21 at 18.09.24.png>)

To access the data from in the Input Event Type view, run for example:

```sql
select * from "projectfortest"."eventstore_dbt-belt-inputET-pro"
```

![](<../../.gitbook/assets/Screenshot 2021-10-21 at 18.16.39.png>)

Now we can model and build the segment query from the Event Store view using standard SQL as well as all PostgreSQL syntax specifics (e.g. [PostgreSQL's JSON syntax](https://www.postgresql.org/docs/12/datatype-json.html)). Later on, we'll use exactly this `SELECT` query to define our DBT Segment Belt. It is also possible to use any data type you need in the resulting table not just `text` as in Granary 1.1 - see e.g. line 6 in the screenshot:

![](<../../.gitbook/assets/Screenshot 2021-10-21 at 18.23.39.png>)

To give you a jumpstart, SQLPad offers four template queries explaining how to model segments on Profile- and Event Store:

![](<../../.gitbook/assets/image (76).png>)

{% hint style="info" %}
Of course it is possible to join various views or tables residing in your schema. To do so, just create multiple input event types and add them to your DBT belt as explained in step 6 below.
{% endhint %}



### Step 6 - Create DBT Belt

{% hint style="info" %}
Under the hood, the dbt-segment Belt type uses the [DBT](https://www.getdbt.com) framework to execute your SQL. You can use all "config" features described in their documentation by adding them to your segment definition.
{% endhint %}

Given the SELECT query modelled in step 5, we can now use the Belt API to create the DBT Belt using following request:

[POST /projects/:project-name/belts](../../developer-reference/api-reference/belt-api.md#create-and-store-a-belt)

where `:project-name` need to be provided as input to the request (we can refer it from step 1).&#x20;

```
{
	"name": "dbt-belt-projectfortest",
	"description": "segment belt - eventstore example",
	"replicas": 1,
	"eventTypes": [
		"dbt-belt-inputET-pro"
	],
	"beltType": "dbt-segment",
	"outputTypes" : [ 
		{
		 "name" : "dbt-belt-outputET-pr",
		 "version" : "latest"
		 } 
	 ],
	"segmentDefinition": "SELECT  created,  \"correlation_id\",    (message -> 'data' -> 0 -> 'ue_pr' -> 'data' -> 'data' -> 'body' -> 'Route' #>>'{}')::text as Route,    (message->'data'->0->'ue_pr'->'data'->'data'->'body'->'Result'#>>'{}')::text as Result ,    (message->'data'->0->'ue_pr'->'data'->'data'->'body'->'Timestamp'#>>'{}')::integer as Timestamp,    (message->'data'->0->'ue_pr'->'data'->'data'->'environment'->'cid'#>>'{}')::text as environment,     (message->'data'->0->'ue_pr'->'data'->'data'->'event'->'source_type'#>>'{}')::text as source_type FROM \"projectfortest\".\"eventstore_dbt-belt-inputET-pro\"",
	"cronExpression": "*/1 * * * *"
}
```

**Remarks on the DBT Belt model:**

1. in the parameter `eventTypes` you need to mention all event type names whose tables/views you use in your SELECT statement (see step 2)
2. in the parameter `outputTypes` you need to mention the one event type created in step 3
3. for the parameter `segmentDefinition`, take the string-escaped query from step 5
4. the parameter `cronExpression` determines the run schedule for the DBT Belt instance and we can see the output event type's table will get created in SQLPad.

The DBT Belt looks like this in Granary UI:

![](<../../.gitbook/assets/image (73).png>)

Once the CRON schedule starts, the following DBT logs confirm a successful execution:

```
 Running with dbt=0.20.1                                                                                                                                                                                               
 Found 1 model, 0 tests, 0 snapshots, 0 analyses, 148 macros, 0 operations, 0 seed files, 0 sources, 0 exposures                                                                                                       
 16:31:14 | Concurrency: 1 threads (target='grnry')                                                                                                                                                                    
 16:31:14 |                                                                                                                                                                                                            
 16:31:14 | Done.                                                                                                                                                                                                        sql statements generated from provided model                                                                                                                                                                         
 ###############################                                                                                                                                                                                       
 SELECT  created,  "correlation_id",    (message -> 'data' -> 0 -> 'ue_pr' -> 'data' -> 'data' -> 'body' -> 'Route' #>>'{}')::text as Route,    (message->'data'->0->'ue_pr'->'data'->'data'->'body'->'Result'#>>'{}') 
 ::text as Result ,    (message->'data'->0->'ue_pr'->'data'->'data'->'body'->'Timestamp'#>>'{}')::integer as Timestamp,    (message->'data'->0->'ue_pr'->'data'->'data'->'environment'->'cid'#>>'{}')::text as environ 
 ment,     (message->'data'->0->'ue_pr'->'data'->'data'->'event'->'source_type'#>>'{}')::text as source_type FROM "projectfortest"."eventstore_dbt-belt-inputET-pro"                                                   
  ###############################                                                                                                                                                                                      
 Running with dbt=0.20.1                                                                                                                                                                                               
 Found 1 model, 0 tests, 0 snapshots, 0 analyses, 148 macros, 0 operations, 0 seed files, 0 sources, 0 exposures                                                                                                       
 16:32:27 | Concurrency: 1 threads (target='grnry')                                                                                                                                                                    
 16:32:27 |                                                                                                                                                                                                            
 16:32:27 | 1 of 1 START view model projectfortest.sgmt_dbt-belt-outputET-pr..... [RUN]                                                                                                                                
 16:32:27 | 1 of 1 OK created view model projectfortest.sgmt_dbt-belt-outputET-pr [CREATE VIEW in 0.07s[]                                                                                                              
 16:32:27 |                                                                                                                                                                                                            
 16:32:27 | Finished running 1 view model in 70.50s.                                                                                                                                                                   
 Completed successfully                                                                                                                                                                                                
 Done. PASS=1 WARN=0 ERROR=0 SKIP=0 TOTAL=1                                                                                                                                                                            
```

The output segment gets created in the database and we can query it in SQLPad:&#x20;

![](<../../.gitbook/assets/Screenshot 2021-10-21 at 18.38.39.png>)
