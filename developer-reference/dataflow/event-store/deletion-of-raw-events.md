# Deletion of Raw Events

## Event Store Reaper Concept

{% hint style="info" %}
The Event Store Reaper concept detailed on this page works independently of the Data-In event type's project scope.
{% endhint %}

In Granary all raw events whose Time-to-Live (TTL) has expired will be deleted from the Event Store by Granary's Event Reaper that runs periodically in the background.&#x20;

There are three options how an event can become TTL-expired:

![](<../../../.gitbook/assets/image (72).png>)

### Source Trigger Delete in Harvester (Option 1)

Source trigger means that the source system sends a deletion event that a Granary Harvester needs to handle. Option 1 is that the Harvester's transformation script marks those events as deletion events. Technically, this adds a Kafka header `grnry-deletion-flag=true` to the event.

An example how to apply this deletion wrapper to an event can be found in the [best practice section](../../../learning-grnry-1/data-in/best-practices-1/logging.md#delete-events).&#x20;

### Source Trigger Delete in Data-In Event Type (Option 2)

Option 2 is to provide a deletion expression written in [SpEL](../../../learning-grnry-1/data-in/best-practices-1/best-practices.md) to the Harvester's Data-In Event Type. If the defined expression evaluates to true, the event is also extended with a Kafka header `grnry-deletion-flag=true`. See [Event Type documentation](../event-type.md) for details.

### Expired Time-to-Live triggers Deletion (Option 3)

Additionally to the above mentioned source trigger options, Granary also offers to define a TTL on Data-In Event Type level that generally applies to all raw events of this type. The Event Reaper collects events where the event type's `eventstoreTTL` has expired since their creation. These events are emitted to the event types corresponding `data_in` topic. The events are also assigned a `grnry-deletion-flag=true` header which acts as a tombstone.

{% hint style="info" %}
In all three options, the deletion event is also consumable from within a use case's Belt. I.e. the Belt can process these deletion notifications and act on them.
{% endhint %}

### How does the physical deletion happen?

When the Event Type's [persister](../data-in/eventstore-sink.md#eventstore-batch-sink) encounters an event with the `grnry-deletion-flag` header set to `true`, it will change the event's TTL in the [database table Event Store](./#table-eventstore) so that the event expires after a short period of time (default: `5 days`). This time is configurable via Harvester API's [update event type persister endpoint](../../api-reference/harvester-api/event-type-endpoints/#update-persister-config-for-a-specific-event-type).

Finally, when the Event Reaper encounters an event whose TTL configured in the database table's column has expired, it is directly deleted from the Event Store without emission of any further messages.

See [TTL-expired Raw Event Processing](../../../learning-grnry-1/data-in/best-practices-1/ttl-expired-raw-event-processing.md) for a practical example of this process.
