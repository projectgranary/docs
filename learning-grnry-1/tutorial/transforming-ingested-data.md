---
description: >-
  In this chapter, we are going to implement and deploy a Belt in order to
  import our first Grains to the Profile Store.
---

# Transforming ingested Data

![](../../.gitbook/assets/grafik%20%2829%29.png)

## 1. Obtain Authentication Token

First step of all API interactions via Postman is to obtain a valid Keycloak token. This token is only valid for 5 minutes. I.e. you need to rerun this API request from time to time.

## 2. Create Belt "Session Counter"

In Granary, a belt is the resource that transforms incoming events in real-time to updates in the Profile Store. Conceptionally, you can perceive belts as a Function as a Service framework.

To run a belt, we need to provide a Python script that transforms the ingested event's metadata \(as defined in our [Event Type](registering-new-data.md#2-create-event-type-customer-session)\) and the event's payload \(as sent in the previous chapter\) to one or many [Profile Update objects](../../developer-reference/dataflow/belt-extractor.md#configuration). Those profile updates are written to the Profile Store. In the Profile Store, each update is a Grain. A Grain is a single piece of information in a profile and follow [this schema](../../developer-reference/dataflow/profile-store/#table-profilestore). 

Most of the belt configuration is covered by defaults, so creating your first belt goes like this:

![Belt API Request: POST /belts](../../.gitbook/assets/image%20%2842%29.png)

For the body \(look for the tab with the green dot\) use this JSON and add your name in line 7 before sending: 

{% hint style="info" %}
The name of the Event Type in line 17 must match the Event Type's `name` you created in chapter "Registering new Data Pipeline".
{% endhint %}

```javascript
{
    "name": "Session Counter",
    "description": "This belt counts the tutorial's sessions.",
    "author": "<your name>",
    "extractorVersion": "0.8.1",    
    "extractorFn": "from grnry.beltextractor.update import Update\r\n\r\ndef execute(event_headers, event_payload, profile=None):\r\n    print(profile)\r\n    # Create Profile Update object with correlationId (String) and path (Array<String>)\r\n    update = Update(event_headers['grnry-correlation-id'],[\"session\", \"counter\"])\r\n    # Set value of Profile Update\r\n    update.set_value(\"0|1|1\")\r\n    # Set type of Profile Update\r\n    update.set_type('sessions')\r\n    # Set operation of Profile Update\r\n    update.set_operation('_inc')\r\n    return [update]",
    "eventTypes": [
        "customer-session"
    ]
}
```

Find more detailed explanation on each belt parameter in the [Belt API reference](../../developer-reference/api-reference/belt-api.md#create-and-store-a-belt).

The extractor function in line 6 of the belt's JSON payload is the Python script mentioned aboved that transforms ingested events to Profile Updates. It looks actually like this:

```python
from grnry.beltextractor.update import Update

def execute(event_headers, event_payload, profile=None):
    print(profile)
    # Create Profile Update object with correlationId (String) and path (Array<String>)
    update = Update(event_headers['grnry-correlation-id'],["session", "counter"])
    # Set value of Profile Update
    update.set_value("0|1|1")
    # Set type of Profile Update
    update.set_type('sessions')
    # Set operation of Profile Update
    update.set_operation('_inc')

    return [update]
```

This Python script simply counts the incoming events per session. Some explanation:

* Line 1 imports the [Profile Update object](../../developer-reference/dataflow/belt-extractor.md#configuration) 
* Line 3 defines the [callback method](../../developer-reference/dataflow/belt-extractor.md#callback-signature) `execute()` which is called by the belt framework for each incoming event and carries the event's header metadata and the payload
* Line 6 creates an Profile Update object with the signature `correlationId` and `path[]`. We can perceive both elements of the signature as the Grain's `key`
* Line 8 sets the Grain's `value`. In our case this is a special value for Granary's [counter operation](../../developer-reference/dataflow/profile-store/#counter). See line 12, where we set the update's operation name to `_inc` \(increment\).

More advanced examples to follow in later chapters. Check this [best practice](../using-data-in-granary/best-practices/easing-development.md) how to transform it to a one-liner.

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "version": "1",
    "name": "Session Counter",
    "description": "This belt counts the tutorial's sessions.",
    "labels": [],
    "affectedPaths": [],
    "replicas": 1,
    "millicpu": 200,
    "memory": 512,
    "author": "<your name>",
    "reader": [
        "_auth"
    ],
    "editor": [
        "belt_edit"
    ],
    "viewer": [
        "belt_view"
    ],
    "created": 1590746652647,
    "assumedRole": "",
    "requirementsPy": "",
    "extractorVersion": "0.8.1",
    "extractorFn": "from grnry.beltextractor.update import Update\r\n\r\ndef execute(event_headers, event_payload, profile=None):\r\n    print(profile)\r\n    # Create Profile Update object with correlationId (String) and path (Array<String>)\r\n    update = Update(event_headers['grnry-correlation-id'],[\"session\", \"counter\"])\r\n    # Set value of Profile Update\r\n    update.set_value(\"0|1|1\")\r\n    # Set type of Profile Update\r\n    update.set_type('sessions')\r\n    # Set operation of Profile Update\r\n    update.set_operation('_inc')\r\n    return [update]",
    "eventTypes": [
        "customer-session"
    ],
    "partitionOffsets": {},
    "kafkaDestinationTopic": "profile-update",
    "beltType": "",
    "runtime": "",
    "parameter": "",
    "debug": false,
    "fetchProfile": "FALSE",
    "profileType": "_d",
    "secret": "",
    "secretUsername": "",
    "secretPassword": "",
    "status": "STOPPED",
    "volumes": null,
    "volumeMounts": null,
    "extraEnv": "{\"extraEnv\": [{\"name\": \"CALLBACK_TIMEOUT_SEC\", \"value\": \"300\"}, {\"name\": \"CALLBACK_LONGRUNNING_SEC\", \"value\": \"180\"}]}",
    "kubernetesName": "grnry-belt-35",
    "id": "35"
}
```

Again, Granary applied a whole bunch of default configuration. Especially, we got an ID \(line 44\) for our belt that we need in the followin step. Next up, starting this belt.

## 3. Start Belt "Session Counter"

Within step 2, we created a belt. In order to be able to transform data, we need to start the belt. This goes like so:

![Belt API: POST /belts/35/state](../../.gitbook/assets/image%20%2840%29.png)

For the body \(look for the tab with the green dot\) use this JSON:

```javascript
{
	"action": "START"
}
```

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "status": "DEPLOYING"
}
```

The deployment goes quite quickly. You can check the current state like so:

![Belt API: GET /belts/35/state](../../.gitbook/assets/image%20%2841%29.png)

With a Status `200 OK`, the return body looks like this:

```javascript
{
    "status": "RUNNING",
    "podStatus": [
        {
            "name": "grnry-belt-35-569f9fd6bb-hsg6q",
            "podEvents": [
                {
                    "lastTransitionTime": "2020-05-29T10:09:36Z",
                    "status": "True",
                    "type": "Initialized"
                },
                {
                    "lastTransitionTime": "2020-05-29T10:10:01Z",
                    "status": "True",
                    "type": "Ready"
                },
                {
                    "lastTransitionTime": "2020-05-29T10:10:01Z",
                    "status": "True",
                    "type": "ContainersReady"
                },
                {
                    "lastTransitionTime": "2020-05-29T10:09:36Z",
                    "status": "True",
                    "type": "PodScheduled"
                }
            ],
            "containerStatus": [
                {
                    "containerID": "docker://ee7237f426a57b68f753e205868ad9b3ff72d69d304d1c86b2e72f00ee7e091f",
                    "image": "hub.syncier.cloud/grnry/belt-extractor:0.8.1",
                    "imageID": "docker-pullable://hub.syncier.cloud/grnry/belt-extractor@sha256:8fd67bf65b27a2028bb2371d6e5abc7ed52db42d2ada49fbd4f3ef568256de3b",
                    "lastState": {},
                    "name": "grnry-belt-35",
                    "ready": true,
                    "restartCount": 0,
                    "state": {
                        "running": {
                            "startedAt": "2020-05-29T10:10:01Z"
                        }
                    },
                    "started": true
                }
            ]
        }
    ]
}
```

