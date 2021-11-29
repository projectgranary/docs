# Live-Segments

Live-Segments can be used from within a Belt to write data directly to a database table given a specified schema in Kafka Schema registry, and persisting the data using Kafka Connect's JDBC sink.

They provide a real time table which is easily accessible in the database. The implementation is simple, whereby we don’t need to write SQL, simply create the [JSON Schema](https://json-schema.org), and construct the messages within our [Python belt code](../belt-extractor.md).

{% hint style="info" %}
Find a step-by-step user guide to create a Live-Segment in the [Learning Granary](../../../learning-grnry-1/get-data-out-of-granary/how-to-model-a-live-segment-with-a-belt.md) section.
{% endhint %}

## Live-Segment Architecture

A Live-Segment consists of multiple components. Most prominently for the user are the event type and the Python Belt.

### Live-Segment Event Type

{% hint style="info" %}
For details on the creation of a Live-Segment Event Type, consult its [reference](../event-type.md#live-segment).
{% endhint %}

The creation of a Live-Segment event types involves a number of services, as shown by the sequence diagram here:

![](https://lh5.googleusercontent.com/1bcbRchXYCtIVfB17egb7GpGx4pln-Op7DeUQcFw8EfGo2LBxJDXOX9cZkk5qsakL5Odekhlif-68jn8STVTO8\_1kgi0RloqwgRTFXk7ks13TnQ0LLvz7vts-AZuUA9mib7N4GxM)

### Kafka Schema Registry

Kafka Schema Registry provides a versioned store of metadata for all Live-eegment event types. It supports the evolution of schemas, and ensures that all schema changes are backward compatible. Consult the official documentation for details:

{% embed url="https://docs.confluent.io/platform/current/schema-registry/index.html" %}

### JSON Schema

Kafka Schema registry is capable to store schemas in Avro, Protobuf, and JSON Schema. Due to incoming data being provided in JSON format, the schema must be provided using the JSON Schema format.&#x20;

{% hint style="info" %}
JSON schema is documented here: [https://json-schema.org/](https://json-schema.org)
{% endhint %}

JSON schema specifies the format for input data, allowing the incoming data to be validated. Properties such as types, and required fields, can be used to ensure the data matches the required format. Multiple types can be provided to achieve e.g. a nullable constrain in SQL:

```json
{
    "$schema": "http://json-schema.org/draft-07/schema",
    "title": "A JSON Schema",
    "type": "object",
    "properties": {
        "id": {
            "type": "integer", ### --> leads to "NOT NULL" column in SQL table
            "examples": [
                1
            ]
        },
        "name": {
            "type": [
                "string", 
                "null"
            ], ### --> leads to optional column in SQL table
            "examples": [
                "A Live-Segment Sample"
            ]
        }
    },
    "additionalProperties": false
}        
```

{% hint style="danger" %}
A Live-Segment's JSON schema must contain a field named `id`. The value of this field will be used as the primary key in the database table. Events without an id column cannot be processed and will be sent to the dead letter queue.
{% endhint %}

Furthermore features such as default values, can also be very useful during schema evolution. However, Kafka Connect does not support a default value on root level:

```json
{
    "$schema": "http://json-schema.org/draft-07/schema",
    "title": "A JSON Schema",
    "type": "object",
    "default": {}, ### --> NOT ALLOWED! Live Segment will fail.
    "properties": {
        "id": {
            "type": "integer",
        },
        "checked": {
            "type": "boolean",
            "default": false ### --> leads to default value in SQL table for this column
        }
    },
    "additionalProperties": false
```

#### Schema Changes & Evolution

Schema evolution allows us to update an existing event type schema (assuming if matches required validation).

On Event Type API `PUT` call the new schema will be validated to ensure it is valid, and ensure it meets `BACKWARD` compatibility with the existing schema versions.

The `BACKWARD` compatibility supports simple change such as:

* Adding a new optional field
* Removing an existing field

\
Further details on schema compatibility are documented here [https://docs.confluent.io/platform/current/schema-registry/avro.html](https://docs.confluent.io/platform/current/schema-registry/avro.html).&#x20;

{% hint style="warning" %}
The document focuses on Avro as the default Schema format, therefore take care to ensure the details also apply to JSON Schema.
{% endhint %}

{% hint style="warning" %}
Live-Segments can be modified as normal event types, this can include providing an updated schema. In order for a belt to start using the schema, the belt must also be updated to the latest event-type version.
{% endhint %}



### Kafka Connect

For Live-Segment, Granary uses Kafka Connect. Please refer to official documentation for general information on Kafka Connect:

{% embed url="https://docs.confluent.io/platform/current/connect/index.html" %}

Kafka connect provides the mapping and persistence of live-segment events. It provides simple mapping from the JSON events on the Kafka topic, to the JSON schema required format, and then the persisting of the data in the Postgres tables.

#### Database Mapping

Live-Segment data is persisted in the database using Kafka Connect JDBC sink.

JDBC sink has knowledge of the schema provided for the event types, allowing the mapping from the incoming JSON event, to the relevant DB tables.

Furthermore table auto creation and evolution is supported, according to a mapping between the provided JSON Schema and the Postgres database columns.

Updates are also possible, by providing the `id` property in the JSON schema, this allows the JDBC Sink to create/update the values for the rows, according to the input PK.

JDBC sink is documented in more detail here:

{% embed url="https://docs.confluent.io/kafka-connect-jdbc/current/sink-connector/index.html#jdbc-sink-data-mapping" %}

#### Configure Kafka Connect task

Granary allows to modify the configuration of the Live-Segment's Kafka Connect JDBC sink to a certain extend. To do so, please refer to the [Event Type Live-Segment Persister API](../../api-reference/harvester-api/event-type-endpoints/live-segment-persisters.md#set-persister) documentation.



### Live-Segment Belt

The Live-Segment belt provides the binding between the input event type (e.g. `data_in`), and the output event type.

{% code title="belt-model.json" %}
```json
{
  "name": "live-segment-belt",
  "eventTypes": [ "live-segment-it-snow"],
  "outputTypes":[ {"name": "live-segment-type","version": "latest"} ],
  ...
}
```
{% endcode %}

The belt contains a callback function that must be written in Python, the function should result in a `DirectUpdate` object being created, with the key being the Live-Segment's technical name.&#x20;

The belt can perform mapping/filtering logic first as required.

{% code title="belt-callback.py" %}
```python
import ...
from grnry_belt.models.direct_update import DirectUpdate

def execute(event_headers, event_payload, profile=None):
    body = lookup(event_payload, ['body'])
    data = lookup(body, ['data'])[0]
    test_data = lookup(body, ['integration_test_data'])[0]
    update = DirectUpdate('live-segment-type', test_data)
    return [update]
```
{% endcode %}



### Postgres

Live-Segment data is stored in Postgres tables, with a separate table for each Live-Segment.

The Live-Segment tables lives inside the containing project schema. See [Project](../../projects.md) reference for details.

Tables are auto created, according to the definitions in the Kafka Schema Registry for that event type. Tables naming is as follows:

`` `grnry_live_sgmt_` + event_type_name ``

\
Privileges for INSERT, UPDATE, SELECT, DELETE will be granted for the project database user.

\
