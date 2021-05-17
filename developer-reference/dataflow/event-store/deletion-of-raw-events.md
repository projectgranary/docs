# Deletion of Raw Events

In GRNRY all events whose Time-to-Live \(TTL\) has expired will be deleted from the Event Store by the GRNRY Event Reaper. 

The Event Reaper collects events where the event type's `eventstoreTTL` has expired since their creation. These events are emitted to the event types corresponding `data_in` topic. The events are also assigned a `grnry-deletion-flag=true` tombstone header which acts as a tombstone.

When the persister encounters an event with the tombstone header set to `true`, it will change the events TTL in the Event Store so that the event expires after a short period of time \(default: `5 days`\). This time is configurable via Harvester API's event type endpoints. See the [EventStore Sink](../data-in/eventstore-sink.md#eventstore-batch-sink) reference for details.

Finally, when the Event Reaper encounters an event whose TTL configured in the database table's column has expired, it is directly deleted from the Event Store without emission of any further messages.

See [TTL-expired Raw Event Processing](../../../learning-grnry-1/data-in/best-practices-1/ttl-expired-raw-event-processing.md) for a practical example of this process.

