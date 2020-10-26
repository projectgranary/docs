# TTL-expired Grain Processing

TTL-expired grains are picked up periodically by Granary's [Reaper ](../profile-store/reaper.md)and written to TTL topics with the following naming schema: `grnry_data_in_<profile-type>`. These grains are then available for further processing by the belts. Profile type here refers to the type of profile, as seen [here](../profile-store/#table-profilestore), with underscores `_`normalized into hyphens `-`. If the resulting topic name exceeds Kafka's limit of 249 characters, it is cut off at the maximum possible length. 

Please note that to comply with data protection law, grains written to the `grnry_data_in_<profile_type>`topics do not carry a value. If the value is needed, one has to query them from the Profile Store from within the belt, i.e setting the `fetch_profile`parameter to true. See [Configuration](./#configuration). The correlation ID should be found in both message header and body.

## Reaper Event Definition

Expired grains in the TTL topics carry the following header values:

```text
topic: grnry_data_in_<profile_type>
kafka_messageKey: <profile_type>
grnry-harvester-name: grnry-reaper
grnry-correlation-id: <correlation_id>
grnry-event-timestamp: reaper processing time timestamp
grnry-event-id: random event UUID
grnry-event-type: <profile-type>
```

and  payload:

```text
String correlationId;
String profileType;
String path;
String pit;
String grain_type;
String inserted;
String ttl;
String reader;
String origin;
```

### JSON Examples

```text
event_headers = {'grnry-harvester-name': 'grnry-reaper', 'grnry-event-type': 'reaper-ttl',
 'grnry-event-id': 'cfcea2b7-68e1-4489-9577-1616179e6235', 'grnry-correlation-id': 'correlationABC',
 'grnry-event-timestamp': '1584708855528'} 

event = {'correlationId': 'correlationABC', 'profileType': 'reaper-ttl', 'path': '/somepath',
 'pit': '_20190926T132523.132Z', 'grain_type': 't', 'inserted': 1569504323132, 'ttl': 'PT30S',
 'reader': '_auth', 'origin': 'harvester-xyz'}

```

## Use Case Example: Notification 

It might be desired to receive notifications about the expiry of certain actions \(grains\). One way to facilitate this is to create a belt, - let's call it a notification belt -, which ingests the TTL-expired grains from the topic\(s\) and sends notifications accordingly.   
It is, however, worth mentioning that since those expired grains are read from the [Profile Store](../profile-store/) and written periodically into the topics, they will appear multiple times if they weren't previously processed, resulting in multiple notifications being dispatched. We therefore advise a notification belt to also trigger a delete operation on the expired grains being processed, to ensure that they will not be read twice.
