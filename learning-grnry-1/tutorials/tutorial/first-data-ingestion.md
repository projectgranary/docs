---
description: >-
  In this chapter, we ingest our first data via the deployed Harvester to the
  Event Store
---

# Ingesting the first Data

![](<../../../.gitbook/assets/grafik (5).png>)

## 1. Send event to Granary's HTTP tracking endpoint

With the harvester in place and deployed, we can now send out first event to Granary's [Snowplow](../../../developer-reference/api-reference/snowplow-api-endpoints.md) HTTP endpoint. For this is a public endpoint, no special authorization is needed. Simply send this event:

![](<../../../.gitbook/assets/image (37).png>)

Snowplow API request:

> **POST /api/com.snowplowanalytics.snowplow/tp2**

For the body (look for the tab with the green dot) use this JSON:

```javascript
{
  "data": [
    {
      "correlationId": "Session4711",
    "filterCriteria": "customer-sessions",
    "start": "2020-05-14T14:05:56",
    "end": "2020-05-14T14:15:42",
    "type": "calculate quote",
    "product": "home contents insurance",
    "context": "size:56sq,value:60.000€",
    "outcome": "contract",
    "device": "Device42",
    "customer": "Customer911",
    "contract": "Contract5432"
    }
 ]
}
```

With a Status `200 OK`, the return body looks like this:

```javascript
ok
```

## 2. Verify successful Data Ingestion

For easy data retrieval, Granary offers an [SQLPad](https://getsqlpad.com/#/) instance as web-based SQL editor for each project.

For `global` project, in your web browser, you can reach it at&#x20;

> **\<yourHostname>/sqlpad/global/queries/new**

For the event type created in the previous step, a view was created:

`"global"."eventstore_cust-sess-<yourName>"`

This view can be easily queried with a SELECT query:

```sql
select * from "global"."eventstore_cust-sess-<yourName>"
```

In the web ui, this looks like this:

![SQLPad web ui](<../../../.gitbook/assets/image (73).png>)

