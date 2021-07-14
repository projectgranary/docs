# TTL-expired Raw Event Processing

In GRNRY during every run of the Event Reaper all events whose TTL has expired are deleted from the Event Store. Furthermore all events where the corresponding Event Type's TTL has expired are collected by the Event Reaper and deletion events are emitted for each of them.

The Event Reaper emits these deletion events into the corresponding Event Type's `grnry_data_in` topic. The deletion events can be distinguished by having a `grnry-deletion-flag` header set to `true`. 

The deletion events adhere to the following schema:

```javascript
event_headers = {
  'grnry-harvester-name': 'grnry-event-reaper',
  'grnry-deletion-flag': 'true',
  'grnry-event-type': 'my-event-type',
  'grnry-event-id': 'cfcea2b7-68e1-4489-9577-1616179e6235',
  'grnry-correlation-id': 'correlationABC',
  'grnry-event-timestamp': '1584708855528'
} 

event = {
  'correlationId': 'correlationABC',
  'eventId: cfcea2b7-68e1-4489-9577-1616179e6235',
  'eventType': 'my-event-type',
  'eventTypeVersion': 1,
  'eventHarvester': 'my-harvester',
  'partitionId': 1,
  'partitionIdOffset': 1
}
```

The correlation ID and event ID headers and body values match the ones of the original event. This allows reacting to the TTL expiry and fetching the original event in user written belts by invoking the event store client function provided in the [belt framework](../../../developer-reference/dataflow/belt-extractor.md#profile-fetching).

