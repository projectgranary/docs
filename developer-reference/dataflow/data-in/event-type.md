---
description: This page introduces the concept of event types.
---

# Event Type

To identify data in the platform, Granary uses so-called event types. An event type describes the metadata and schema of a particular category of raw data. Event Types are defined by their name and a group of rules specifying how the incoming data is annotated and handled in Granary's components. These rules are denoted in the [Spring Expression language](../../../learning-grnry-1/data-in/best-practices-1/best-practices.md).

A minimal sample for an event type looks like this:

```javascript
{
	"displayName": "my-event-type",
	"eventIdExpression": "#randomUUID()",
	"correlationIdExpression": "#safeJsonPath(payload, 'my_id')?:'NO_CORRELATION_ID'"
}
```

Most of the event type's parameters are assigned default values, you are only required to set the parameters seen above. To get more insight into all of the configuration parameters and their defaults, please refer to the [Event Type Endpoints](../../api-reference/harvester-api/event-type-endpoints.md) page.

Based on the event types configuration parameters the [Metadata Extractor](metadata-extractor.md) will enrich the incoming events with headers. 

An Event Type is referenced in a Harvester like this:

```javascript
{
   ...
   "eventType": {
    "name": "my-event-type",
    "version": "latest"
   
  },
  ...
}
```

