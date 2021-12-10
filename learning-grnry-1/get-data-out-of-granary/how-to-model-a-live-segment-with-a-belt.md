---
description: This page is to explain the steps for the use case with live segment.
---

# How to model a Live Segment with a Belt

### Goal

Goal of this guide is to explain Granary's feature to write events directly to a database table given a specified schema. To do so, we need to create a Python Belt emitting events to a Live-Segment event type. The following paragraphs provide details on the steps needed to achieve this goal.

### Step 1 - Create Project (optional)

_Only carry out this step if you do not have a Granary project yet. For details refer to the_ [_Create Project in Granary_](../create-project-in-granary.md) _guide._



### Step 2 - Create Input Event Type

Belts writing a Live-Segment require one or many input Event Types that reference a stream location. Currently, Granary supports the following Event Types as input for Live-Segment Belts:

* [Data-In](../../developer-reference/dataflow/event-type.md#data-in)
* [Time-To-Live](../../developer-reference/dataflow/profile-store/reaper.md)
* [Time-To-Notify](../../developer-reference/dataflow/profile-store/reaper.md)

To create the input Event Type, we either use Granary UI or the Harvester API with the following request:

[POST /projects/:project-name/event-types](../../developer-reference/api-reference/harvester-api/event-type-endpoints/#create-an-event-type)

where `:project-name` needs to be provided as input to the request (we can refer it from step 1).

```json
{
    "displayName": "livesegment-datain",
    "description": "Live Segment DataIn Event Type",
    "type": "data_in",
    "eventIdExpression": "#randomUUID()",
    "timestampExpression": "#nowMillis()",
    "correlationIdExpression": "#randomUUID()",
    "eventstoreTTL": "P100Y"
}
```

The Data-In Event Type looks like this in Granary UI:

![](<../../.gitbook/assets/Screenshot 2021-11-09 at 14.49.08.png>)

Please notice the "Show Event Browser" toggle that opens the event browser to view all events on this Data-In Types stream location.&#x20;

{% hint style="info" %}
In case of Data-In and Live-Segment, please ensure that their persisters (right node in the graph) have the state running. Otherwise, no data will be written to the database.
{% endhint %}



### Step 3 - Create Output Event Type

To get a Live Segment up and running, this step is the most crucial one. Belts that write a Live-Segment require an output [Event Type of type `live_segment`](../../developer-reference/dataflow/event-type.md#live-segment). This Event Type defines the Live-Segment database table's structure by supplying a [JSONSchema](https://json-schema.org). In your Belt code (see step 5), you need to model a JSON output that matches schema provided in this step.

{% hint style="info" %}
Your are not limited to a single `live_segment` event type per belt, it's also possible to write to multiple event types from a single phyton-belt, as long as these output event types are all of type `live_segment` or `data_in.`.
{% endhint %}

Therefore, we need to create an output Event Type using Granary UI or&#x20;

[POST /projects/:project-name/event-types](../../developer-reference/api-reference/harvester-api/event-type-endpoints/#create-an-event-type)

where `:project-name` needs to be provided as input to the request (we can refer it from step 1). For the `schema` parameter, provide your string-escaped JSONSchema.&#x20;

```json
{
    "displayName": "livesegment-dataoutput",
    "type": "live_segment",
    "schema": {
        "schema": "{\r\n    \"type\": \"object\",\r\n    \"title\": \"The root schema\",\r\n    \"description\": \"This is a sample json for testing the live segment.\",\r\n    \"required\": [\r\n        \"id\",\r\n        \"firstName\",\r\n        \"lastName\"\r\n    ],\r\n    \"properties\": {\r\n        \"id\": {\r\n            \"type\": \"integer\"\r\n        },\r\n        \"firstName\": {\r\n            \"type\": \"string\",\r\n            \"default\": \"325\"\r\n        },\r\n        \"lastName\": {\r\n            \"type\": \"string\",\r\n            \"default\": \"325\"\r\n        }\r\n    },\r\n    \"additionalProperties\": true\r\n}"
    }
}
```

Be aware that in the later steps the technical name of the event type is always used. If you do not explicitly define the name by yourself on event type creation, it will be derived from the display name. In this example the technical name was shorted to `livesegment-dataoutp`.

{% hint style="info" %}
[https://jsonschema.net/home](https://jsonschema.net/home) provides an easy-to-use tool to model your JSONSchema. No need to write the model yourself!
{% endhint %}

The Live-Segment Event Type looks like this in Granary UI:&#x20;

![](<../../.gitbook/assets/Screenshot 2021-11-09 at 15.54.58.png>)

The configuration including the JSONSchema looks like this:

![](<../../.gitbook/assets/Screenshot 2021-11-09 at 15.56.41.png>)

If using the Granary UI to create the Live-Segment event type, it provides a JSONSchema validator.  Whenever you paste a valid JSONSchema, it will be pretty-printed automatically. Potential errors are hightlighted as such and an error description is given.

In our guide, we use this JSONSchema:

```json
{
    "type": "object",
    "title": "The root schema",
    "description": "This is a sample json for testing the live segment.",
    "required": [
        "id",
        "firstName",
        "lastName"
    ],
    "properties": {
        "id": {
            "type": "integer"
        },
        "firstName": {
            "type": "string",
            "default": "325"
        },
        "lastName": {
            "type": "string",
            "default": "325"
        }
    },
    "additionalProperties": true
}
```

{% hint style="info" %}
Be aware that each schema needs to include a mandatory field named "id" of type string.
{% endhint %}

### Step 4 - Create Harvester&#x20;

To be able to model a Live-Segment in the Belt, we need to ensure that there is a Harvester serving our input event type(s) with data. Follow the [Harvester creation guide](../data-in/how-to-run-a-harvester/harvesters.md) and reference the input Event Type created in step 2. Do not forget to start the Harvester after creation:

```json
{
    "displayName": "harvesterforlivesegment",
    "sourceType": {
        "name": "grnry-topic",
        "version": "latest"
    },
    "transform": {
        "app": "grnry-scriptable",
        "version": "latest",
        "language": "groovy",
        "script": "import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe\\nimport groovy.json.JsonSlurper\\n\\nSnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)\\nbody = snowplowEvent.getBody()\\n\\nJsonSlurper slurper = new JsonSlurper()\\nevent = slurper.parseText(body)\\n\\nif (event?.appId=='liveSegmenttest') {\\n\\n return body\\n}\\n\\nreturn null"
    },
    "eventType": {
        "name": "livesegment-datain",
        "version": "1"
    }
}
```

![](<../../.gitbook/assets/Screenshot 2021-11-10 at 10.50.22.png>)

After that, check the Event Type from step 2 in Granary UI again and see the Harvester depicted as input node:

![](<../../.gitbook/assets/Screenshot 2021-11-10 at 10.52.28.png>)



### Step 5 - Create a Python Belt

As stated before, in the Python Belt's callback function, we need to write the mapping logic from incoming events to the JSONSchema defined in step 3 so that the output events can be written to the database correctly.

Create the Belt either using Granary UI or by calling:

[POST /projects/:project-name/belts](../../developer-reference/api-reference/belt-api.md#create-and-store-a-belt)

where `:project-name` needs to be provided as input to the request (we can refer it from step 1).

The request for the belt creation looks like:

```json
{
  "name": "liveSegement-belt",
  "beltType": "python-callback",
  "eventTypes": [
    "livesegment-datain"
  ],
  "outputTypes": [
    {
      "name": "livesegment-dataoutp",
      "version": "latest"
    }
  ],
  "extractorFn": "import json\r\nimport sys\r\nfrom time import time\r\nimport logging\r\nimport math\r\nfrom grnry_belt.models.direct_update import DirectUpdate\r\n\r\ndef lookup(dictionary,keys):\r\n    if type(dictionary)==type(''):\r\n        dictionary=json.loads(dictionary)\r\n    try:\r\n        if len(keys)>1:\r\n            value = lookup(dictionary[keys[0]],keys[1:])\r\n        else:\r\n            value = dictionary[keys[0]]\r\n        return value\r\n    except:\r\n        return None\r\n\r\ndef execute(event_headers, event_payload, profile = None):\r\n    update = []\r\n    correlation_id= event_headers['grnry-correlation-id']    print(\"event_payload : \", event_payload)\r\n    directValue = {\r\n    'id': str(correlation_id),\r\n    'firstName':  str(event_payload['firstName']),\r\n    'lastName':  str(event_payload['lastName'])\r\n    }\r\n    directUpdate = DirectUpdate('testlivesegment-0000',directValue)\r\n    print(\"directUpdate : \", directUpdate)\r\n    update.append(directUpdate)\r\n    logging.debug('Updated the value to the test live segment')\r\n    return update",
  ( ... redacted unimportant belt configuration ..)
} 
```

The python callback code:&#x20;

{% code title="" %}
```python
import json
import sys
from time import time
import logging
import math
from grnry_belt.models.direct_update import DirectUpdate

def execute(event_headers, event_payload, profile = None):
    update = []
    correlation_id= event_headers['grnry-correlation-id']
    directPayload = {
    'id': str(correlation_id),
    'firstName':  str(event_payload['firstName']),
    'lastName':  str(event_payload['lastName'])
    }
    directUpdate = DirectUpdate('livesegment-dataoutp',directPayload)
    update.append(directUpdate)
    return update
```
{% endcode %}

where&#x20;

* lines 11-15 define the Python dict that matches the JSONSchema from step 3
* line 16 instantiates the `DirectUpdate` class that is responsible for wrapping the payload together with the name of the target event type (of type live\_segment)
* line 17 appends the `DirectUpdate` to the update array that we return in line 19.

You need to make sure that the event type name defined in line 16 matches the technical name you already provided in your belt definition in `outputTypes`. This seems like an overhead if you only define a single output type, but the framework allows for issuing multiple updates in the same belt, so you can also define multiple `DirectUpdates` in the scope of a single input message. You can even mix the `DirectUpdates` with the already known `Updates` to simultaneously write to a live segment and to a profile. You just need to add all updates to your updates array. Don't worry about order.&#x20;

Be aware that your payload (constructed in line 11-15 and wrapped as a `DirectUpdate` in line 16) will NOT be checked against your schema within the belt framework. The framework will only expect a field called "id" of type string (mandatory in all schemas) for routing purposes. So, any error related to a mismatch between your payload and the schema will only be visible in the persister of the live segment, NOT in the log of the belt. If activated, the persister itself has a dead letter queue topic where these messages will be put into.

The belt framework will check the technical name of the event type you are targeting in your DirectUpdate  (`livesegment-dataoutp` in this example) and will throw an error if it's not part of your `outputTypes` list or if it's not an `live_segment`.

As mentioned before, your schema and your data need to contain a field named `id`. The value of this field will be used as the primary key, so issuing a second `DirectUpdate` with a previously used `id` will result in an UPDATE of the corresponding row in the live segment's table. Currently there is no functionalty available to DELETE a row in the live\_segment table.

In Granary UI, ensure the belt is created and started:&#x20;

![](<../../.gitbook/assets/Screenshot 2021-11-11 at 11.00.07.png>)



### Step 6 - Send & access Data&#x20;

With step 4 completed, the entire data pipeline to let your Live-Segment write data to the database table defined via the JSONSchema, is up and running. Send test data to Granary's Snowplow endpoint:

[POST /api/com.snowplowanalytics.snowplow/tp2](../../developer-reference/api-reference/snowplow-api-endpoints.md#tracker-endpoint-post)

Try to send the message with the defined parameters from the mentioned JSONSchema from Live-Segment event type:

```json
{
    "appId": "liveSegmenttest",
    "firstName": "Andrew",
    "lastName": "Peterson"
}
```

The Live-Segment table `"grnry_live_segment_livesegment-dataoutp"` gets created and then we can check the data get inserted in the table using Granary's SQL console SQLPad. Open the SQLPad UI from your project via the context menu of the Event Type in Granary UI.&#x20;

![](<../../.gitbook/assets/Screenshot 2021-11-11 at 12.00.56.png>)

In SQLPad, we can see the schema created for your project using the `:project-name` that we specified in step 1. From the editor in the center of the screen, we can access the Live Segment table in our project's the schema:

```
```

