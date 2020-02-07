# Event Types

## Event Type definition

An Event Type describes a dedicated data type that is consumed by belts and imported by harvesters. It has a unique name and contains rules on how to extract important metadata \(correlationId/eventId\) from the event. 

## Creating an Event Type

Creating an event type is the initial step to import data into grnry. 

{% hint style="info" %}
If you want to create a harvester, you need to create the event type that the harvester will import and also register a [source type](source-types.md) implementation.
{% endhint %}

#### name

The name of an event-type can only consist of the characters `a-z` , `A-Z` , `0-9` and `-` . It length can not exceed x chars due to technical limitations.

#### correlationIdExpression

A springEL rule to extract the correlation id from the event \(or create it using a function\).

#### eventIdExpression

A springEL rule to extract the event id from the event \(or create it using a function\).

#### timestampExpression \(optional\)

A springEl Rule to extract the timestamp \(milliseconds since 1.1.1970 UTC\). Optional, if not set the current time is used.

#### Example Call

```text
curl -X POST -H "Content-Type: application/json" \
 -d @event-type.json \
 https://grnry-host/event-types


event-type.json content:

{
  "name" : "test-event-type-post",
  "correlationIdExpression" : "#jsonPath(#jsonPath(payload, 'body'), 'data[0].correlationId')",
  "eventIdExpression" : "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].eventId')?:#randomUUID()",
  "timestampExpression" : "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].timestamp')?:#nowMillis()"
}
```

## Updating an Event Type

Existing event types can be updated to reflect changes to the event type structure that may require changes to the SpringEL expressions to determine the correlationId/eventId etc. Each update will be stored as a new  version  \(autoincremented number starting at 1\).  

The definition of a harvester will point to a single explicit version of an event type and use these expressions to extract metadata \(correlationId/eventId\). 

{% hint style="info" %}
The harvester can use an alias \("latest"\), which will always point to the current version. The extraction rules of that version are read and used on harvester **startup**. So if you update an event type, these updates values are not automatically applied to already running harvesters that are configured to use "latest". You would need to stop and start the harvester so it re-reads the extraction rules.
{% endhint %}

TODO - links to Rest api doc when the api is merged

