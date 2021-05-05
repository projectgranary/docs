---
description: >-
  In this chapter, we ingest our first data via the deployed Harvester to the
  Event Store
---

# Ingesting the first Data

![](../../../.gitbook/assets/grafik%20%285%29.png)

## 1. Send event to Granary's HTTP tracking endpoint

With the harvester in place and deployed, we can now send out first event to Granary's [Snowplow](../../../developer-reference/api-reference/snowplow-api-endpoints.md) HTTP endpoint. For this is a public endpoint, no special authorization is needed. Simply send this event:

![Snowplow API: POST /api/com.snowplowanalytics.snowplow/tp2](../../../.gitbook/assets/image%20%2837%29.png)

For the body \(look for the tab with the green dot\) use this JSON:

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

### Using Event Store API

For retrieving raw events, Granary ships an API called [Event Store API](../../../developer-reference/api-reference/event-store-api.md). To interact with it, you need a authentication token.

To verify that our event above made it to the event store, run this API call:

![Event Store API: GET /events/Session4711](../../../.gitbook/assets/image%20%2817%29.png)

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "events": [
        {
            "eventId": "223865dd-68eb-491a-b61b-5d70bf31004b",
            "correlationId": "Session4711",
            "created": "2020-05-07T11:38:29.717Z",
            "eventType": "customer-session",
            "eventTypeVersion": 1,
            "eventHarvester": "snowplow-customer-se",
            "_links": {
                "self": {
                    "href": "https://demo.grnry.io/events/Session4711/223865dd-68eb-491a-b61b-5d70bf31004b"
                }
            }
        }
    ],
    "_links": {
        "self": {
            "href": "https://demo.grnry.io/events/Session4711?from=1970-01-01T00:00:00Z&to=2038-01-01T00:00:00Z&expand=&offset=0&pagesize=20{&type}",
            "templated": true
        }
    }
}
```

To receive the full message, you must add the query parameter `expand=message`.

{% hint style="warning" %}
If you named the harvester or the event type differently, you will most likely receive a Status `403 Forbidden`. If so, please open a support request to grant you the needed access rights.
{% endhint %}

### Using Metabase

If you prefer a more UI guided option, head over to `<hostname>/metabase/` and press on the "show data" button on the right hand side of the header menu.

To check if our event made it to the raw event store, go this path:

> Postgres -&gt; public -&gt; Events Tore

and filter the event type column for our event type `customer-session`:

![](../../../.gitbook/assets/image%20%2830%29.png)

That's it. You learned how to ingest simple events via Granary's Snowplow HTTP endpoint. Next up is the tutorial how to apply business logic on each of these raw events and create inter-linked profiles.

