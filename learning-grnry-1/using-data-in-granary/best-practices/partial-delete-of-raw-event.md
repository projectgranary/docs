---
description: >-
  This article demonstrates how to mask or partially delete of an event in Event
  Store
---

# Masking or Partial Deletion of Raw Events

Masking or partial deletion of a raw event in Event Store means to delete the original event and create a new event with pseudonymized original information. This process helps to protect the personal information of the user. The flow of the data is shown in the sketch below.

![](../../../.gitbook/assets/image%20%288%29.png)

### Step 1: Create the Snowplow Harvester.

Any existing Snowplow Harvester will do. If you don't have one, create a new one as shown in image below. 

![](../../../.gitbook/assets/image%20%2864%29.png)

{% hint style="info" %}
The process is valid for every type of Harvester supported by Granary. Not only Snowplow ones.
{% endhint %}

### 

### Step 2: Start the Harvester as shown in the image.

Start the harvester by using the drop down at right most corner shown in image.

![](../../../.gitbook/assets/image%20%2863%29.png)

### 

### Step 3: Create an Event Type with short time-to-live.

The Event Type must be of type `data_in` \(default\) and set the TTL to 5 seconds \(`PT5S`\). Start the persister.

We use this Event Type to quickly generate events that become TTL outdated so the [Event Store's deletion](../../../developer-reference/dataflow/event-store/deletion-of-raw-events.md) mechanisms start to work.

![](../../../.gitbook/assets/image%20%2866%29.png)

![](../../../.gitbook/assets/image%20%2867%29.png)

![](../../../.gitbook/assets/image%20%2868%29.png)



### Step 4: Create an Event Type with long time-to-live

The Event Type must be of type `data_in` \(default\) and set the TTL to 1 year  \(`P1Y`\). Start the persister.

We use this Event Type to store the then pseudonymized events from the Event Type created in Step 3.

![](../../../.gitbook/assets/image%20%2869%29.png)

![](../../../.gitbook/assets/image%20%2859%29.png)

![](../../../.gitbook/assets/image%20%2861%29.png)

### 

### Step 5: Create a belt which subscribes to Event Type with short TTL

Create a belt which subscribes the Event Type with TTL of 5 seconds. Direct the outgoing data to the Event Type which was created in Step 4. In the UI, ou can do this by setting the belt's Kafka destination topic to the long-TTL Event Type's topic name. To find this name, look for the property `topicName` in the long-TTL Event Type's export view.

Deletion notification events from the Event Store are indicated by the `grnry_deletion_flag`. See [TTL-expired Raw Event Processing](../../data-in/best-practices-1/ttl-expired-raw-event-processing.md) article for details.

So, in the belt script check for this `grnry_deletion_flag` header \(line 14\). If the value is `true` then process the data. Fetch the sensitive data from the Event Store API \(line 21\) and pseudonymize the information using appropriate algorithm. See `setPayload` in line 28.

Once done, use the Belt framework's Generic Update output format. You need to set all the raw events headers as defined in [Belt framework's input data format](../../../developer-reference/dataflow/belt-extractor.md#input-data-format). Most of the headers are part of the `grnry_deletion_flag = true` event as defined in the before-mentioned [TTL-expired Raw Event processing](../../data-in/best-practices-1/ttl-expired-raw-event-processing.md) guide. See `setHeaders` in line 27.

```python
import hmac
import hashlib
import binascii
import json
import datetime;
from grnry_belt.models import generic_update

def execute(event_headers, event_payload, profile=None):

    updates = []
    profileCorrId=''
    deletionFlag=event_headers['grnry-deletion-flag']
    if deletionFlag:
        		           
            profileCorrId = event_headers['grnry-correlation-id']
            eventId=event_headers['grnry-event-id']
            
            eventres = eventstoreClient.getEvent(profileCorrId,eventId)
            userName = eventres.get('message').get('body').get('data')[0].get('name')
            age = eventres.get('message').get('body').get('data')[0].get('age')
            gender = eventres.get('message').get('body').get('data')[0].get('gender')

            eventHeaders=setHeaders(event_headers,event_payload)
            eventPayload=json.dumps(setPayload(profileCorrId,str(userName),str(age),str(gender)))

            updates.append(generic_update.GenericUpdate(profileCorrId,eventPayload,eventHeaders))
            
            return updates

def setHeaders(event_headers,event_payload):
    harvesterName = event_payload['eventHarvester']
    eventType = 'pseudoeventdata' # must match the Event Type's name from Step 4
    grnryEventId = event_headers['grnry-event-id']
    grnryCorelationId = event_headers['grnry-correlation-id']
    ts = datetime.datetime.now().timestamp()
    grnryTimestamp = int(ts) 
    # alternatively, we can take the original timestamp and convert it to UNIX millis
    # grnryTimestamp = eventres.get('created')
     
    eventHeader= {"grnry-harvester-name": str(harvesterName), "grnry-event-type": str(eventType), "grnry-correlation-id": str(grnryCorelationId), "grnry-event-timestamp": str(grnryTimestamp), "grnry-event-id": str(grnryEventId), "grnry-event-type-version": "1"}

    return eventHeader

def setPayload(correlationId,userName,age,gender):
    pseudoCorrelId = (signature("E49756B4C8FAB4E48222A3E7F3B97CC3", correlationId))
    pseudoUserName = (signature("E49756B4C8FAB4E48222A3E7F3B97CC3", userName))
    seudoAge = (signature("E49756B4C8FAB4E48222A3E7F3B97CC3", age))
    pseudoGender = (signature("E49756B4C8FAB4E48222A3E7F3B97CC3", gender))
 
    eventPayload = {"correlationId": str(pseudoCorrelId), "name": str(pseudoUserName), "age": str(pseudoAge), "gender": str(pseudoGender)}

    return eventPayload

def signature(key, msg):
    key = binascii.unhexlify(key)
    msg = msg.encode()
    update= hmac.new(key, msg, hashlib.sha256).hexdigest().upper()

    return update
```

![](../../../.gitbook/assets/image%20%2850%29.png)

### 

### Step 6: Send the request to the Harvester

```python
{
  "data": [
	  {
	    "correlationId": "maskrawevent",
      "name":"Markas",
      "age":"32",
      "gender": "Female" 
	  }
  ]
}

```



### Step 7: Check the eventstore database to see all the message stored with the topic

![](../../../.gitbook/assets/image%20%2856%29.png)

