---
description: >-
  On this page we explain how to use logging & deletion in the scriptable
  transform.
---

# Scriptable Transform: Logging & Deletion

## Log Events

All transformation scripts utilise the GRNRY logger. The GRNRY logger will output all its messages with the common log format as specified [here](../../../operator-reference/site-reliability/common-log-format.md). Any logs you create by importing the `java.util.logging.Logger` inside the script will appear in the message part of the common GRNRY log format. Below you can find an example what logging in a groovy script could look like:

```groovy
import groovy.json.JsonBuilder
import groovy.json.JsonSlurper
import groovy.json.JsonOutput
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import java.util.logging.Logger


Logger logger = Logger.getLogger('testLogger')
logger.info('An example log.')
```

{% hint style="danger" %}
Please note that if you write messages to STDOUT \(e.g. via the print\(\)-method\), your messages won't be in the common GRNRY logformat and thus will not contain additional information beside the message itself.
{% endhint %}

## Delete Events

Another use case of the script could be the deletion of events from the eventstore table. To mark events for deletion you can return an instance of `io.grnry.scdfapps.scriptable.DeletionEvent` from your script. The constructor expects a `correlationId` and an `eventId` \(of type `String`\) for identifying a single event. If you want to delete all events with a common `correlationId`, there is an additional constructor expecting the `correlationId`  only.

{% tabs %}
{% tab title="Groovy" %}
```groovy
import io.grnry.scdfapps.scriptable.DeletionEvent
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import groovy.json.JsonSlurper

SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)
eventString = SnowplowPayloadExtractor.getActualSnowplowPayload(snowplowEvent)
JsonSlurper slurper = new JsonSlurper()
event = slurper.parseText(eventString)
result = null

print(eventString)
//delete single event
if(event.body && event.body.data && event.body.data[0] && event.body.data[0].correlationId && event.body.data[0].eventId) {
    result = new DeletionEvent(event.body.data[0].correlationId, event.body.data[0].eventId)
}
//delete all events by correlationId
else if(event.body && event.body.data && event.body.data[0] && event.body.data[0].correlationId) {
    result = new DeletionEvent(event.body.data[0].correlationId)
}
return result

```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
var DeletionEvent = Packages.io.grnry.scdfapps.scriptable.DeletionEvent
var serde = Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe; 
var extractor = Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor; 
var collectorPayload = Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload; 

eventString = extractor.getActualSnowplowPayload(serde.deserialize(payload)); 
event = JSON.parse(eventString); 
var result = null; 

//delete single event
if (event.body && event.body.data && event.body.data[0] && event.body.data[0].correlationId && event.body.data[0].eventId) { 
    result = new DeletionEvent(event.body.data[0].correlationId, event.body.data[0].eventId) 
}
//delete all events by correlationId
else if (event.body && event.body.data && event.body.data[0] && event.body.data[0].correlationId) { 
    result = new DeletionEvent(event.body.data[0].correlationId)
}

result;
```
{% endtab %}
{% endtabs %}

If you still want the complete event structure to be processed by further components, you can return a list containing both, the `DeletionEvent` and the `eventString`.

{% tabs %}
{% tab title="Groovy" %}
```groovy
import io.grnry.scdfapps.scriptable.DeletionEvent
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import groovy.json.JsonSlurper

SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)
eventString = SnowplowPayloadExtractor.getActualSnowplowPayload(snowplowEvent)
JsonSlurper slurper = new JsonSlurper()
event = slurper.parseText(eventString)
result = []

print(eventString)
//add deletion object for event
if(event.body && event.body.data && event.body.data[0] && event.body.data[0].correlationId && event.body.data[0].eventId) {
    result.add(new DeletionEvent(event.body.data[0].correlationId, event.body.data[0].eventId))
}
//add event
result.add(event)
return result
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
var serde = Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe; 
var extractor = Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowPayloadExtractor; 
var collectorPayload = Packages.io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload; 

eventString = extractor.getActualSnowplowPayload(serde.deserialize(payload)); 
event = JSON.parse(eventString); 
var result = []; 

//add deletion object for event
if (event.body && event.body.data && event.body.data[0] && event.body.data[0].correlationId && event.body.data[0].eventId) { 
    result.push(JSON.parse(new DeletionEvent(event.body.data[0].correlationId, event.body.data[0].eventId).toJSON()));
}
//add event
result.push(event);

JSON.stringify(result);
```
{% endtab %}
{% endtabs %}



