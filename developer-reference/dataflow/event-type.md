---
description: This page introduces the concept of event types.
---

# Event Types

### Overview

To identify data in the platform, Granary uses so-called event types. An event type describes the metadata or schema of a data asset before and after a transformation. Event Types are defined by a globally unique name and are created via the [Event Type endpoints](broken-reference).

In a Granary data pipeline, the input and the output of each transform step is denoted as one or many event types. Tranform steps can either be a [Python Callback Belt](belt-extractor.md) or a [DBT Segment Belt](segment-store/segment-table-creation.md). An exception to this rule is Harvesters which import raw data and, thus, only define an event type as their output.

To control the data flow, Granary comes with different kinds of event types that can be used at different stages of a data pipeline. These are described in detail in the following.&#x20;

In their JSON response models, all kinds of event types contain information on the data asset's physical location and the transform steps writing / reading them, e.g.:

```json
{
    "name": "User-Visits-5",
    ...
    "sources": [
        {
            "type": "database",
            "connectionSecretRef": "demoproject-pg-credentials",
            "source": "postgres"
        },
        {
            "type": "stream",
            "connectionSecretRef": "grnry-kafka-credentials",
            "source": "kafka"
        }
    ],
    "physicalLocations": [
        {
            "locationName": "grnry_live_segment_User-Visits-5",
            "source": "kafka",
            "type": "stream"
        },
        {
            "locationName": "demoproject.grnry_live_sgmt_User-Visits-5",
            "source": "postgres",
            "type": "database"
        }
    ],
    "inputs": {
        "harvesters": [],
        "belts": []
    },
    "outputs": {
        "persisters": [],
        "belts": []
    }
}
```

As shown above, an event type can have mulitple physical locations. Currently, most event types in Granary have two of them (Segment only one):

1. `Kafka Stream` --> It describes the topic name where the event type's streaming data is stored at. The streaming storage is transient, so after a certain retention time, data is deleted.
2. `Postgres Database` --> it describes the relation (table or view) where the event type's persistent data is stored.

For a general overview, this diagram below visualizes all existing event types and their physical locations.

![](../../.gitbook/assets/grnry\_event\_types.png)

### Data-In

A Data-In event type is a group of rules specifying how a Harvester annotates incoming streaming data with metadata. handled in Granary's components. These rules are denoted in the [Spring Expression language](../../learning-grnry-1/data-in/best-practices-1/best-practices.md).

The minimal sample for a Data-In event type looks like this:

```javascript
{
	"displayName": "my-event-type",
	"eventIdExpression": "#randomUUID()",
	"correlationIdExpression": "#safeJsonPath(payload, 'my_id')?:'NO_CORRELATION_ID'"
}
```

Most of the event type's parameters are assigned default values, you are only required to set the parameters seen above, however, there are two more metadata extraction expressions possible:

| Attribute                 | Description                                                                                                                                                      |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `displayName`             | The display name of an event type. See [hints](../../learning-grnry-1/data-in/best-practices-1/hints-on-naming-of-harvesters-and-event-types.md) on naming.      |
| `correlationIdExpression` | A SpringEL rule to extract the correlation id from the event (or create it using a function). Granary-header: `grnry-correlation-id`.                            |
| `eventIdExpression`       | A SpringEL rule to extract the event id from the event (or create it using a function). Granary-header: `grnry-event-id`.                                        |
| `timestampExpression`     | A SpringEL rule to extract the timestamp (milliseconds since 1.1.1970 UTC). _Optional_, if not set, the current time is used. Granary-header: `grnry-timestamp`. |
| `deletionExpression`      | A SpringEL rule to extract the boolean flag if this is a delete event. _Optional_, if not set it will not be applied. Granary-header: `grnry-deletion-flag`.     |
| `description`             | Description of this specific event type. _Optional_, default is `""`.                                                                                            |

To get more insight into all of the configuration parameters and their defaults, please refer to the [Event Type Endpoints](../api-reference/harvester-api/event-type-endpoints/) page.

Data-In event types come with a [Persister ](../../learning-grnry-1/data-in/how-to-run-a-harvester/event-types.md#eventstore-persisters)that is created during event type creation by the Harvester API. As the name says, it persists the incoming streaming data to the PostrgeSQL database. The default Persister is the [Eventstore Batch Sink](data-in/eventstore-sink.md#eventstore-batch-sink). Also during creation, a view on the [Eventstore table](event-store/#table-eventstore) for the particular Data-In event type is created.&#x20;

Most importantly, Data-In event type's streaming data can be consumed by Python Callback Belts and also written by them to enable Belt chaining.

#### Input/Output Overview

This table describes where the Data-in event type can be used and which location type is used.

| Input                                  | Output                                 |
| -------------------------------------- | -------------------------------------- |
| Python Callback Belt (stream location) | Harvester (stream location)            |
| DBT Segment Belt (database location)   | Python Callback Belt (stream location) |



### Profile

Profile event types represent the profile types in the Profile Store on Granary's API layer. Their streaming data are [Profile Updates](belt-extractor.md#profile-update) emitted by Python Callback Belts. To perform such an update, instantiate an object of the `Update` class and set the profile type via `set_type()` function. A Belt can define multiple Profile outputs at a time.

[Kafka Profile Updater component](profile-store/#component-profile-updater) reads these Profile Update operations and updates the Profile Store - the PostgreSQL database location of Profile event types.&#x20;

The minimal sample of a Profile event type looks like this:

```json
{
  "displayName":"my-profile-type",
  "type" : "profile"
}
```

During creation, a view on the [Profilestore table ](profile-store/#table-profilestore)for the particular Profile event type is created.

{% hint style="info" %}
For existing profile types in Profile Store, the corresponding Profile event type's `displayName` must exactly match.&#x20;

See the [Granary 1.2 migration guide section](../../operator-reference/migration-guide/projects-use-case-migration-concept.md#new-type-profile) on Profile event types for more details.
{% endhint %}

#### Input/Output Overview

This table describes where the Profile event type can be used and which location type is used.

| Input                                | Output                                 |
| ------------------------------------ | -------------------------------------- |
| DBT Segment Belt (database location) | Python Callback Belt (stream location) |



### Live-Segment

Live-Segment event types can be used to write streaming data directly to a PostgreSQL database table given a specified schema using Kafka Connect's JDBC sink. To do so, an object of the `DirectUpdate` class in the Python Callback Belt needs to be instantiated.

The event type's creation triggers the initialization of the Kafka connector. After creation, the connector it is automatically started. By default the connector has three task. This can be changed over the [Persister endpoint](broken-reference).&#x20;

The minimal sample for a Live-Segment event type looks like this:

```json
{
    "displayName":"my-live-segment-event-type",
    "type" : "live_segment",
    "schema": {
      "subject" :"test-event",
      "schema": "{"type":"object","properties":{"id":{"description":"PK","type":"integer"},"test":{"type":["null","string"],"description":"Data"}}}"
  }   
}
```

Before deployment the schema can be validated on [https://www.jsonschemavalidator.net/](https://www.jsonschemavalidator.net) . Every schema has to specify a property `id`. This field is used as a primary key to update and delete existing entries.

#### Input/Output Overview

This table describes where the Live-Segment event type can be used and which location type is used.

| Input                                | Output                                 |
| ------------------------------------ | -------------------------------------- |
| DBT Segment Belt (database location) | Python Callback Belt (stream location) |



### Segment

The Segment event type is used for meta information on DBT Segment belts. Its name is used as table name in the PostgeSQL database.&#x20;

The minimal sample for a Segment event type looks like this:

```json
{
  "displayName":"my-segment-event-type",
  "type" : "segment"
}
```

#### Input/Output Overview

This table describes where the Segment event type can be used and which location type is used.

| Input                                | Output                               |
| ------------------------------------ | ------------------------------------ |
| DBT Segment Belt (database location) | DBT Segment Belt (database location) |
