---
description: Springboot-based microservice to manage belts registered in the Belt Store.
---

# Belt API

## Paths

* GET /belts
* GET /belts/{beltId}
* POST /belts
* DELETE /belts/{beltId}
* PUT /belts/{beltId}
* GET/belts/{beltId}/state
* POST/belts/{beltId}/state

## API Endpoints

{% api-method method="get" host="https://api.grnry.io" path="/belts" %}
{% api-method-summary %}
Get all Belts
{% endapi-method-summary %}

{% api-method-description %}
Retrieves full dump of **all** belts in the Belt Store as a list.  
  
In order to get results, you must have the required roles as defined in the fields _editor_ or _viewer_. Otherwise, you will not get back any results.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="expand" type="array" required=false %}
array of belt states:  
\* Running  
\* Stopped  
\* Running\_but\_outdated  
\* Deploying  
\* Failed
{% endapi-method-parameter %}

{% api-method-parameter name="pagesize" type="string" required=false %}
Number of belts to be returned. Default is 20.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="string" required=false %}
Start offset. Default is 0.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A list of all belts along with their attributes and total count of belts stored in the Belt Store.
{% endapi-method-response-example-description %}

```
{
    "totalCount": 502,
    "_links": {
        "next": {
            "href": "https://development.lce.grnry.io/belts?offset=2&pagesize=2&expand="
        },
        "self": {
            "href": "https://development.lce.grnry.io/belts?offset=0&pagesize=2&expand="
        }
    },
    "items": [
        {
            "version": "1",
            "name": "kube_test_4",
            "kubernetesName": "grnry-belt-kube-test-4",
            "description": "",
            "labels": [
                "a"
            ],
            "affectedPaths": [],
            "replicas": 1,
            "millicpu": 200,
            "memory": 512,
            "author": "",
            "reader": [
                "_auth"
            ],
            "editor": [
                "belt_edit"
            ],
            "viewer": [
                "belt_view"
            ],
            "created": 1561974412801,
            "assumedRole": "",
            "requirementsPy": "",
            "extractorVersion": "feature-scrum-229-metadata-header",
            "extractorFn": "from time import time\r\nfrom grnry.beltextractor.update import Update\r\n\r\ndef execute(event, profile=None):\r\n    print(profile)\r\n    update = Update(profile['correlationId'],[\"dummy\"]).set_value(\"Hallo Belt!\",0.5,time(),'P1D','Dummy-Belt')\r\n    update.set_type('TestProfileType')\r\n    return [update]\r\n",
            "eventTypes": [
                "test_event",
                "test_event_alt"
            ],
            "partitionOffsets": {
                "grnry_data_in_test_event": [
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0
                ],
                "grnry_data_in_test_event_alt": [
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0
                ]
            },
            "beltType": "",
            "runtime": "",
            "parameter": "",
            "debug": true,
            "fetchProfile": true,
            "profileType": "_d",
            "secret": "belt-api-tester",
            "secretUsername": "username",
            "secretPassword": "password",
            "status": "RUNNING",
            "_links": {
                "self": {
                    "href": "https://development.lce.grnry.io/belts/1011"
                }
            },
            "id": "1011"
        },
        {
            "version": "1",
            "name": "kube_test_7",
            "kubernetesName": "grnry-belt-kube-test-7",
            "description": "kube_description",
            "labels": [
                "a",
                "b",
                "c"
            ],
            "affectedPaths": [
                "a",
                "b",
                "c"
            ],
            "replicas": 1,
            "millicpu": 54,
            "memory": 1023,
            "author": "author@grnry.com",
            "reader": [
                "_auth"
            ],
            "editor": [
                "belt_edit"
            ],
            "viewer": [
                "belt_view"
            ],
            "created": 1561978858485,
            "assumedRole": "assumeRole",
            "requirementsPy": "requirementsPy",
            "extractorVersion": "latest",
            "extractorFn": "print('hallo welt')",
            "eventTypes": [
                "eventType1",
                "eventType2",
                "eventType3"
            ],
            "partitionOffsets": {
                "grnry_data_in_eventTypes": [
                    20
                ]
            },
            "beltType": "customScript",
            "runtime": "Python",
            "parameter": "{mapper:[{\"beltId\",\"ida\"}]}",
            "debug": true,
            "fetchProfile": true,
            "profileType": "_d",
            "secret": "secret",
            "secretUsername": "secretUsername",
            "secretPassword": "secretPassword",
            "status": "STOPPED",
            "_links": {
                "self": {
                    "href": "https://development.lce.grnry.io/belts/1022"
                }
            },
            "id": "1022"
        }
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/belts/:beltId" %}
{% api-method-summary %}
Get a Specific Belt by ID
{% endapi-method-summary %}

{% api-method-description %}
Retrieves full dump of a specific belt with a specified ID.  
  
In order to retrieve results here, it is necessary that you either have an _editor_ or _viewer_ role assigned to your profile in keycloak. The editor/viewer role must match the roles defined for the belt.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="beltId" type="string" required=true %}
The ID of belt
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication Token
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
JSON with attributes of belt with the specified ID.
{% endapi-method-response-example-description %}

```
{
    "beltId": "3",
    "version": "3",
    "name": "test_log",
    "description": "kube_description_updated_3",
    "labels": [],
    "affectedPaths": [],
    "replicas": 0,
    "inputTopic": [],
    "millicpu": 0,
    "memory": 0,
    "author": "",
    "reader": [],
    "editor": [
        "belt_edit"
    ],
    "viewer": [
        "belt_view"
    ],
    "created": 1556236800000,
    "assumedRole": "",
    "requirementsPy": "",
    "extractorVersion": "",
    "extractorFn": "",
    "eventTypeFilter": "",
    "beltType": "",
    "runtime": "",
    "parameter": "",
    "debug": true,
    "fetchProfile": true,
    "secret": "secret_updated",
    "secretUsername": "secretUsername_updated",
    "secretPassword": "secretPassword_updated",
    "startOffset": "25",
    "status": "STOPPED",
    "_links": {
        "self": {
            "href": "https://development.lce.grnry.io/belts/3"
        }
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
When there is no belt found with the specified ID
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.grnry.io" path="/belts" %}
{% api-method-summary %}
Create and Store a Belt
{% endapi-method-summary %}

{% api-method-description %}
Creates and stores a belt into the Belt Store. As a response the whole belt configuration is returned.   
  
In order to create / update / delete a belt here, it is necessary that you have a _viewer_ role assigned to your profile in keycloak. The editor role must match the roles defined for the belt.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="profileType" type="string" required=false %}
Profile type to fetch default is `_d`. Only used if \``fetchProfile` is set to true
{% endapi-method-parameter %}

{% api-method-parameter name="partitionOffsets" type="object" required=false %}
A mapping from input topics to respective start offsets which are provided as an array with the indices corresponding to the partition numbers. Requires a replica count of `1` at most.
{% endapi-method-parameter %}

{% api-method-parameter name="extractorVersion" type="string" required=false %}
Image tag of the belt runtime docker image to be used: default is `latest`
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
Belt name: Needs to be unique
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
Belt description
{% endapi-method-parameter %}

{% api-method-parameter name="labels" type="array" required=false %}
String array of labels
{% endapi-method-parameter %}

{% api-method-parameter name="affectedPaths" type="array" required=false %}
String array of affected profile paths
{% endapi-method-parameter %}

{% api-method-parameter name="replicas" type="integer" required=false %}
Number of replicas; cannot be greater than `1` if `partitionOffsets` is set: Default is `1`
{% endapi-method-parameter %}

{% api-method-parameter name="eventTypes" type="array" required=false %}
String array of event types to be processed
{% endapi-method-parameter %}

{% api-method-parameter name="millicpu" type="string" required=false %}
Deployment specification for this belt: Default is `200` 
{% endapi-method-parameter %}

{% api-method-parameter name="memory" type="string" required=false %}
Deployment specification for this belt: Default is `512`
{% endapi-method-parameter %}

{% api-method-parameter name="author" type="string" required=false %}
Author of this belt
{% endapi-method-parameter %}

{% api-method-parameter name="reader" type="array" required=false %}
String array containing read permissions of profiles to be read/modified by this belt. Default to `{"_all"}`
{% endapi-method-parameter %}

{% api-method-parameter name="editor" type="array" required=false %}
String array containing Keycloak roles given permission to edit this belt. Default to `{"belt_edit"}`
{% endapi-method-parameter %}

{% api-method-parameter name="assumedRole" type="string" required=false %}
Assumed roles
{% endapi-method-parameter %}

{% api-method-parameter name="requirementsPy" type="string" required=false %}
`requirements.txt` for belts with python runtime
{% endapi-method-parameter %}

{% api-method-parameter name="extractorFn" type="string" required=false %}
extractor function to be executed by this belt
{% endapi-method-parameter %}

{% api-method-parameter name="beltType" type="string" required=false %}
belt type
{% endapi-method-parameter %}

{% api-method-parameter name="runtime" type="string" required=false %}
runtime of this belt.
{% endapi-method-parameter %}

{% api-method-parameter name="viewer" type="array" required=false %}
String array containing keycloak roles. The role\(s\), which should have read access to the belt. Default to `{"belt_view"}`. 
{% endapi-method-parameter %}

{% api-method-parameter name="debug" type="boolean" required=false %}
if belt should be run in debug mode: Default is `false`.
{% endapi-method-parameter %}

{% api-method-parameter name="fetchProfile" type="boolean" required=false %}
if belt should fetch profiles from the Profile Store
{% endapi-method-parameter %}

{% api-method-parameter name="secret" type="string" required=false %}
Secret name as deployment specification for this belt
{% endapi-method-parameter %}

{% api-method-parameter name="secretUsername" type="string" required=false %}
Username set in the secret
{% endapi-method-parameter %}

{% api-method-parameter name="secretPassword" type="string" required=false %}
Password set in the secret
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns a full dump of belt object created.
{% endapi-method-response-example-description %}

```
{
    "beltId": "27",
    "version": "1",
    "name": "test-post-23",
    "description": "",
    "labels": [],
    "affectedPaths": [],
    "replicas": 0,
    "eventTypes": [],
    "millicpu": 0,
    "memory": 0,
    "author": "",
    "reader": [
        "_auth"
    ],
    "editor": [
        "belt_edit"
    ],
    "created": 1557100800000,
    "assumedRole": "",
    "requirementsPy": "",
    "extractorVersion": "",
    "extractorFn": "",
    "beltType": "",
    "runtime": "",
    "parameter": "",
    "debug": false,
    "fetchProfile": false,
    "secret": "",
    "secretUsername": "",
    "secretPassword": "",
    "partitionOffsets": {},
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If a belt with a given name exists already in the Belt Store.
{% endapi-method-response-example-description %}

```
{
    "timestamp": "2019-05-06T09:54:54.074+0000",
    "message": "test-post-24 : test-post-24 exists already",
    "details": "uri=/belts"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host="https://api.grnry.io" path="/belts/:beltId" %}
{% api-method-summary %}
Delete a Specific Belt
{% endapi-method-summary %}

{% api-method-description %}
Deletes a specific belt given the ID.  
  
In order to create / update / delete a belt here, it is necessary that you have a _viewer_ role assigned to your profile in keycloak. The editor role must match the roles defined for the belt.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="beltId" type="string" required=true %}
Belt ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication Token
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
If delete is successful, content=true. content=false when unsuccessful
{% endapi-method-response-example-description %}

```
{
    "content": true
}

or 

{
    "content": false
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
No belt found with given ID
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="put" host="https://api.grnry.io" path="/belts/:beltId" %}
{% api-method-summary %}
Updates a Belt by ID
{% endapi-method-summary %}

{% api-method-description %}
Updates attributes of a belt, given its ID.   
  
For a list of body parameters to send, see above POST /belts documentation. All parameters would be optional. One could update one or multiple attributes at the same time. Please note, that you need to provide all belt details in case of a put. Otherwise data might get lost.  
  
**Important: In the current implementation, belt version will automatically increase by each update.**  
  
In order to create / update / delete a belt here, it is necessary that you have a _viewer_ role assigned to your profile in keycloak. The editor role must match the roles defined for the belt.  
  
Body examples:   
  
`{"name": "belt-updated-1"}`  
or   
  
`{"name": "belt-updated-1", "beltType": "someBeltType"}`
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="beltId" type="string" required=true %}
Belt ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication Token
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A full dump of belt object recently modified
{% endapi-method-response-example-description %}

```
{
    "beltId": "15",
    "version": "4",
    "name": "test-put-5",
    "description": "",
    "labels": [],
    "affectedPaths": [],
    "replicas": 0,
    "inputTopic": [],
    "millicpu": 0,
    "memory": 0,
    "author": "",
    "reader": [],
    "editor": [
        "belt_edit"
    ],
    "viewer": [
        "belt_view"
    ],
    "created": 1557100800000,
    "assumedRole": "",
    "requirementsPy": "",
    "extractorVersion": "",
    "extractorFn": "",
    "eventTypeFilter": "",
    "beltType": "",
    "runtime": "",
    "parameter": "",
    "debug": false,
    "fetchProfile": false,
    "secret": "",
    "secretUsername": "",
    "secretPassword": "",
    "startOffset": "0"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Belt with the given ID not found
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### 

{% api-method method="get" host="https://api.grnry.io" path="/belts/:beltId/state" %}
{% api-method-summary %}
Get a Belt's state
{% endapi-method-summary %}

{% api-method-description %}
Retrieve the status of the Belt's Kubernetes deployment.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="beltId" type="string" required=true %}
Belt ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "status": "STOPPED"
}

 or
  
{
    "status": "FAILED",
    "deploymentReplicas": "1",
    "deploymentEvents": [
        {
            "lastTransitionTime": "2019-07-01T11:02:38Z",
            "lastUpdateTime": "2019-07-01T11:02:38Z",
            "message": "Deployment does not have minimum availability.",
            "reason": "MinimumReplicasUnavailable",
            "status": "False",
            "type": "Available"
        },
        {
            "lastTransitionTime": "2019-07-01T11:12:39Z",
            "lastUpdateTime": "2019-07-01T11:12:39Z",
            "message": "ReplicaSet \"grnry-belt-kube-test-5-54f5c68c89\" has timed out progressing.",
            "reason": "ProgressDeadlineExceeded",
            "status": "False",
            "type": "Progressing"
        }
    ],
    "podStatus": [
        {
            "name": "grnry-belt-kube-test-5-54f5c68c89-8xf9v",
            "podEvents": [
                {
                    "lastTransitionTime": "2019-07-01T11:02:38Z",
                    "status": "True",
                    "type": "Initialized"
                },
                {
                    "lastTransitionTime": "2019-07-01T11:02:38Z",
                    "message": "containers with unready status: [grnry-belt-kube-test-5]",
                    "reason": "ContainersNotReady",
                    "status": "False",
                    "type": "Ready"
                },
                {
                    "lastTransitionTime": "2019-07-01T11:02:38Z",
                    "message": "containers with unready status: [grnry-belt-kube-test-5]",
                    "reason": "ContainersNotReady",
                    "status": "False",
                    "type": "ContainersReady"
                },
                {
                    "lastTransitionTime": "2019-07-01T11:02:38Z",
                    "status": "True",
                    "type": "PodScheduled"
                }
            ],
            "containerStatus": [
                {
                    "image": "gitlab.alvary.io:5000/grnry/belt-extractor:extractorVersion",
                    "imageID": "",
                    "lastState": {},
                    "name": "grnry-belt-kube-test-5",
                    "ready": false,
                    "restartCount": 0,
                    "state": {
                        "waiting": {
                            "message": "rpc error: code = Unknown desc = Error: image grnry/belt-extractor:extractorVersion not found",
                            "reason": "ErrImagePull"
                        }
                    }
                }
            ]
        }
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=302 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Possible values for the status attribute in the response body

| Status | Description |
| :--- | :--- |
| RUNNING | Belt is running |
| FAILED | Belt is deployed but not running |
| RUNNING\_BUT\_OUTDATED | Belt is running but there is a newer version of it in the database |
| DEPLOYING | Belt is being deployed |
| STOPPED | Belt is not deployed |

{% api-method method="post" host="https://api.grnry.io" path="/belts/:beltId/state" %}
{% api-method-summary %}
Manipulate a Belt's state
{% endapi-method-summary %}

{% api-method-description %}
Update the status of the Belt's Kubernetes deployment.  
  
Body example:  
`{"action": "START"}`  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="beltId" type="string" required=true %}
Belt ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="action" type="string" required=true %}
The action to be performed "START" or "STOP"
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
    "status": "DEPLOYING"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

