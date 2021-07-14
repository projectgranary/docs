---
description: >-
  This page describes how to write a script that transforms snowplow-events into
  corresponding grnry-data-in json-documents and filters them by content.
---

# Scriptable Transform:  Process Snowplow Events

## Transform Thrift to Json

In the preceding section Getting Started there is a listing containing a call to SCDF. This is where the properties are specified that SCDF will apply to apps in the pipeline. Among others, there is the `app.grnry-scriptable.scriptable-transformer.script` property. Its value is a `String` containing a groovy script that transforms incoming events. For Snowplow events, one important part is the message conversion from `thrift` format to `json` format. The classes needed for deserialization are already contained in the `grnry-scriptable` app. These are `io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe` and `io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload`.

{% tabs %}
{% tab title="Groovy" %}
```groovy
import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload

//deserialized payload
SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)​

//json String representation of payload with escaped body
eventString = SnowplowSerDe.toJSON(snowplowEvent)

return eventString
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
var serde=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe;
var extractor=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor;
var collectorPayload=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload;

serde.toJSON(serde.deserialize(payload));
```
{% endtab %}
{% endtabs %}

## Normalize Json Representation of SnowplowEvents

The body of a Snowplow event is stored in a String and therefore will be escaped in the json representation of the object. To get a normalized hence unescaped version you should use the helper method `SnowplowPayloadExtractor.normalizeJson()`.

{% tabs %}
{% tab title="Groovy" %}
```groovy
import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import com.fasterxml.jackson.databind.ObjectMapper

//deserialized payload
SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)​

//json String representation of payload with escaped body
eventString = SnowplowSerDe.toJSON(snowplowEvent)

//normalized (unescaped) json node representation of snowplow event
normalizedEventString = SnowplowPayloadExtractor.normalizeJson(eventString)

return normalizedEventString
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
var serde=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe;
var extractor=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor;
var collectorPayload=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload;

eventString = serde.toJSON(serde.deserialize(payload));

extractor.normalizeJson(eventString);
```
{% endtab %}
{% endtabs %}

## Normalize and add GET Parameters

If you expect any GET parameters the helper method `SnowplowPayloadExtractor.getActualSnowplowPayload()` will, after normalizing your event, add them to the `queryParameters` node of the resulting json String. Each parameter key will contain a list of parameter values.

{% tabs %}
{% tab title="Groovy" %}
```groovy
import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import com.fasterxml.jackson.databind.ObjectMapper

//deserialized payload
SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)​

//get normalized json String representation of snowplow event with GET parameters at node "queryParameters"
eventString = SnowplowPayloadExtractor.getActualSnowplowPayload(snowplowEvent)

return eventString
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
var serde=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe;
var extractor=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor
var collectorPayload=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload;

eventString = serde.toJSON(serde.deserialize(payload));

extractor.getActualSnowplowPayload(eventString);
```
{% endtab %}
{% endtabs %}

## Access Nested Values of Event Representations

{% tabs %}
{% tab title="Groovy" %}
```groovy
import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import com.fasterxml.jackson.databind.ObjectMapper
import groovy.json.JsonSlurper

//deserialized payload
SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)

//get normalized json String representation of snowplow event with GET parameters at node "queryParameters"
eventString = SnowplowPayloadExtractor.getActualSnowplowPayload(snowplowEvent)

JsonSlurper slurper = new JsonSlurper()

//Object representation of normalized payload
event = slurper.parseText(eventString)

//use dot operator and array index access to get an object at a specific path in the event
ue_pr = event.body.data[0].ue_pr

return SnowplowSerDe.toJSON(ue_pr)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
var serde=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe;
var extractor=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor;
var collectorPayload=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload;

eventString = serde.toJSON(serde.deserialize(payload));

eventString = extractor.getActualSnowplowPayload(eventString);
event = JSON.parse(eventString);
serde.toJSON(event.body.data[0].ue_pr);
```
{% endtab %}
{% endtabs %}

## Filter Events

Most of the time you don't want all events to pass the same pipeline. So for each pipeline you provide some filter criteria. The following script will search the POST-body of the received snowplow event for the value of `data.[0].filtercriteria` and its GET parameters for the value of `filterCriteria[0]` and only forwards it if it matches `"your-filter"`. Otherwise the event is skipped.

{% tabs %}
{% tab title="Groovy" %}
```groovy
import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import com.fasterxml.jackson.databind.ObjectMapper
import groovy.json.JsonSlurper

//deserialized payload
SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)

//get normalized json String representation of snowplow event with GET parameters at node "queryParameters"
eventString = SnowplowPayloadExtractor.getActualSnowplowPayload(snowplowEvent)

JsonSlurper slurper = new JsonSlurper()

//Object representation of normalized payload
event = slurper.parseText(eventString)

if (event?.body?.data?.filterCriteria) {
    //find filterCriteria in POST body
    if (event.body.data[0].filterCriteria.equalsIgnoreCase("your-filter")) {
        return eventString
    }
}
if (event?.queryParameters?.filterCriteria){
    //find filterCriteria in GET query params
    if (event.queryParameters.filterCriteria[0].equalsIgnoreCase("your-filter")) {
        return eventString
    }
}

// skip event
return null
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
var serde=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe;
var extractor=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor;
var collectorPayload=Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload;

eventString = extractor.getActualSnowplowPayload(serde.deserialize(payload));
event = JSON.parse(eventString);
result = null;

if (event.body && event.body.data && event.body.data[0] 
        && event.body.data[0].filterCriteria) {
    //find filterCriteria in POST body
    if (event.body.data[0].filterCriteria.equalsIgnoreCase("your-filter")) {
        result = eventString;
        }
}
if (event.queryParameters && event.queryParameters.filterCriteria 
        && event.queryParameters.filterCriteria[0]){
    //find filterCriteria in GET query params
    if (event.queryParameters.filterCriteria[0].equalsIgnoreCase("your-filter")) {
        result = eventString;
    }
}
result;
```
{% endtab %}
{% endtabs %}

## 

