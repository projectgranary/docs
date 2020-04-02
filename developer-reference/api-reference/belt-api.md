---
description: Springboot-based microservice to manage belts registered in the Belt Store.
---

# Belt API

## Paths

* GET /belts
* GET /belts/{id}
* POST /belts
* DELETE /belts/{id}
* PUT /belts/{id}
* GET/belts/{id}/state
* POST/belts/{id}/state

## API Methods

Consult the [Granary Access Clients Reference](../../operator-reference/identity-and-access-management/granary-access-clients.md#belt-api) for roles a user needs to interact with Belt API.

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
{% api-method-parameter name="search" type="string" required=false %}
Filter belts by name. Belt names need to be url encoded. Default "".
{% endapi-method-parameter %}

{% api-method-parameter name="expand" type="array" required=false %}
Array of belt states. For possible values, see table at GET belt state definition below.
{% endapi-method-parameter %}

{% api-method-parameter name="pagesize" type="string" required=false %}
Number of belts to be returned. Default is `20`. Maximum is `250`.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="string" required=false %}
Start offset. Default is `0`.
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
            "href": "https://hostname/belts?offset=2&pagesize=2&expand="
        },
        "self": {
            "href": "https://hostname/belts?offset=0&pagesize=2&expand="
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
                "test-a",
                "test-b"
            ],
            "partitionOffsets": {
                "grnry_data_in_test-a": [
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
                "grnry_data_in_test-b": [
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
            "kafkaDestinationTopic": "profile-update",
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
                    "href": "https://hostname/belts/1011"
                }
            },
            "id": "1011"
        },
        {
            "version": "1",
            "name": "kube_test_7",
            "kubernetesName": "grnry-belt-kube-test-7",
            "description": "kube_description",
            "labels": [],
            "affectedPaths": [],
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
            "assumedRole": "",
            "requirementsPy": "requirement==0.1.0",
            "extractorVersion": "latest",
            "extractorFn": "print('hallo welt')",
            "eventTypes": [
                "eventType1",
                "eventType2",
                "eventType3"
            ],
            "partitionOffsets": {},
            "kafkaDestinationTopic": "profile-update",
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
                    "href": "https://hostname/belts/1022"
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

{% api-method method="get" host="https://api.grnry.io" path="/belts/:id" %}
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
{% api-method-parameter name="id" type="string" required=true %}
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
    "version": "1",
    "name": "hello-belt",
    "kubernetesName": "grnry-belt-hello-belt",
    "description": "Hello Belt Belt",
    "labels": [],
    "affectedPaths": [],
    "replicas": 1,
    "millicpu": 200,
    "memory": 512,
    "author": "User",
    "reader": [
        "\"_auth\""
    ],
    "editor": [
        "belt_edit"
    ],
    "viewer": [
        "belt_view"
    ],
    "created": 1562744768164,
    "assumedRole": "",
    "requirementsPy": "package1==0.0.0\r\npackage2",
    "extractorVersion": "0.5.0",
    "extractorFn": "from time import time\r\nfrom grnry.beltextractor.update import Update\r\n\r\ndef execute(headers, event, profile=None):\r\n print(profile)\r\n update = Update(headers['grnry-correlation-id'],[\"dummy\"]).set_value(\"Hallo Belt!\",0.5,time(),'P1D','Dummy-Belt')\r\n update.set_type('TestProfileType')\r\n return [update]\r\n",
    "eventTypes": [
        "test-a",
        "test-b"
    ],
    "partitionOffsets": {},
    "kafkaDestinationTopic": "profile-update",
    "beltType": "",
    "runtime": "",
    "parameter": "",
    "debug": false,
    "fetchProfile": "FALSE",
    "profileType": "test-type",
    "secret": "",
    "secretUsername": "",
    "secretPassword": "",
    "status": "STOPPED",
    "volumes": null,
    "volumeMounts": null,
    "extraEnv": null,
    "id": "161",
    "_links": {
        "self": {
            "href": "https://hostname/belts/161"
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
  
**JSON Schema Definitions**  
  
Volume Mount: https://javadoc.io/doc/io.fabric8/kubernetes-model/3.0.1/io/fabric8/kubernetes/api/model/VolumeMount.html  
  
Volume: https://javadoc.io/doc/io.fabric8/kubernetes-model/3.0.1/io/fabric8/kubernetes/api/model/Volume.html  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="kafkaDestinationTopic" type="string" required=false %}
Provide a different destination topic for this belt as the default. Defaults to Belt API Server setting for destination topic. Defaults to either `profile-update` or server env variable `BELT_DESTINATION_TOPIC`.
{% endapi-method-parameter %}

{% api-method-parameter name="extraEnv" type="object" required=false %}
Additional environment variables for Kubernetes belt deployment. Defaults to either `null` or server env variable `BELT_EXTRA_ENV`.
{% endapi-method-parameter %}

{% api-method-parameter name="volumeMounts" type="object" required=false %}
JSON Kubernetes volume mount definition for belt deployment. Defaults to either `null` or server env variable `BELT_VOLUME_MOUNTS`.
{% endapi-method-parameter %}

{% api-method-parameter name="volumes" type="object" required=false %}
JSON Kubernetes volume definition for belt deployment. Defaults to either `null` or server env variable `BELT_VOLUMES`.
{% endapi-method-parameter %}

{% api-method-parameter name="requirementsPy" type="string" required=false %}
PIP package requirements for belt. E.g. `package1==0.1.0\r\npackage2`
{% endapi-method-parameter %}

{% api-method-parameter name="profileType" type="string" required=false %}
Profile type to fetch. Defaults to `_d`. **Required when fetchProfile is `TRUE`.**
{% endapi-method-parameter %}

{% api-method-parameter name="partitionOffsets" type="object" required=false %}
Mapping from input topics to respective start offsets which are provided as an array with the indices corresponding to the partition numbers. The offset settings only have an effect on the first start of a Belt. All subsequent \(re-\)starts of configuration update will read the event type topics from consumer group's current offset.
{% endapi-method-parameter %}

{% api-method-parameter name="extractorVersion" type="string" required=false %}
Image tag of the belt runtime docker image to be used. Defaults to either `latest` or server env variable `BELT_EXTRACTOR_VERSION`.
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
Unique name of the belt.
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
Belt description.
{% endapi-method-parameter %}

{% api-method-parameter name="replicas" type="integer" required=false %}
Number of replicas. Defaults to `1`
{% endapi-method-parameter %}

{% api-method-parameter name="eventTypes" type="array" required=false %}
String array of event types to be processed. Only event types registered with Harvester API's event type endpoint are valid values.
{% endapi-method-parameter %}

{% api-method-parameter name="millicpu" type="string" required=false %}
Deployment specification for this belt. Defaults to either `200` or server env variable `BELT_MILLI_CPU`.
{% endapi-method-parameter %}

{% api-method-parameter name="memory" type="string" required=false %}
Deployment specification for this belt. Defaults either to `512` or server env variable `BELT_MEMORY`.
{% endapi-method-parameter %}

{% api-method-parameter name="author" type="string" required=false %}
Author of this belt.
{% endapi-method-parameter %}

{% api-method-parameter name="editor" type="array" required=false %}
String array containing Keycloak roles given permission to edit this belt. Default to `["belt_edit"]`
{% endapi-method-parameter %}

{% api-method-parameter name="extractorFn" type="string" required=false %}
Extractor function to be executed by this belt.  
Defaults either to `Hello World` example function or server env variable `BELT_EXTRACTOR_FN`.
{% endapi-method-parameter %}

{% api-method-parameter name="viewer" type="array" required=false %}
String array containing keycloak roles. The role\(s\), which should have read access to the belt. Defaults to `["belt_view"]`. 
{% endapi-method-parameter %}

{% api-method-parameter name="debug" type="boolean" required=false %}
Determines if belt should be run in debug mode. Defaults to `false`.
{% endapi-method-parameter %}

{% api-method-parameter name="fetchProfile" type="string" required=false %}
Determines if belt should fetch profiles from the Profile Store. Defaults to `FALSE`. Possible values: `["FALSE", "TRUE", "LAZY"]`
{% endapi-method-parameter %}

{% api-method-parameter name="secret" type="string" required=false %}
Kubernetes Secret name used by belt to read values from Profile Store. **Required when fetchProfile is `TRUE`or `LAZY`**_**.**_
{% endapi-method-parameter %}

{% api-method-parameter name="secretUsername" type="string" required=false %}
Username property in secret used by belt to read values from Profile Store.  **Required when fetchProfile is `TRUE` or `LAZY`.**
{% endapi-method-parameter %}

{% api-method-parameter name="secretPassword" type="string" required=false %}
Password property in secret used by the belt to read values from Profile Store. **Required when fetchProfile is `TRUE` or `LAZY`.**
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
    "version": "1",
    "name": "hello-belt",
    "kubernetesName": "grnry-belt-hello-belt",
    "description": "Hello Belt Belt",
    "labels": [],
    "affectedPaths": [],
    "replicas": 1,
    "millicpu": 200,
    "memory": 512,
    "author": "User",
    "reader": [
        "\"_auth\""
    ],
    "editor": [
        "belt_edit"
    ],
    "viewer": [
        "belt_view"
    ],
    "created": 1562744768164,
    "assumedRole": "",
    "requirementsPy": "package1==0.0.0\r\npackage2",
    "extractorVersion": "0.5.0",
    "extractorFn": "from time import time\r\nfrom grnry.beltextractor.update import Update\r\n\r\ndef execute(headers, event, profile=None):\r\n print(profile)\r\n update = Update(headers['grnry-correlation-id'],[\"dummy\"]).set_value(\"Hallo Belt!\",0.5,time(),'P1D','Dummy-Belt')\r\n update.set_type('TestProfileType')\r\n return [update]\r\n",
    "eventTypes": [
        "test-a",
        "test-b"
    ],
    "partitionOffsets": {},
    "kafkaDestinationTopic": "profile-update",
    "beltType": "",
    "runtime": "",
    "parameter": "",
    "debug": false,
    "fetchProfile": "FALSE",
    "profileType": "test-type",
    "secret": "",
    "secretUsername": "",
    "secretPassword": "",
    "status": "STOPPED",
    "volumes": "{\"volumes\": [{\"name\": \"test\", \"configMap\": {\"name\": \"log-config\", \"items\": [{\"key\": \"log_level\", \"path\": \"log_level\"}]}}]}",
    "volumeMounts": "{\"volumeMounts\": [{\"name\": \"test\", \"subPath\": \"\", \"readOnly\": true, \"mountPath\": \"/etc/test\", \"secretName\": \"test-certs\"}]}",
    "extraEnv": "{\"extraEnv\": [{\"name\": \"FOO\", \"value\": \"bar\"}]}",
    "id": "161"
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

{% api-method method="delete" host="https://api.grnry.io" path="/belts/:id" %}
{% api-method-summary %}
Delete a Specific Belt
{% endapi-method-summary %}

{% api-method-description %}
Deletes a specific belt given the ID.  
  
Deletes the belt definition in the database and all corresponding kubernetes resources if the belt is deployed.  
  
The Keycloak user needs to have the _editor_ roles defined for this Belt.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
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
{ "content": true } or { "content": false }
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

{% api-method method="put" host="https://api.grnry.io" path="/belts/:id" %}
{% api-method-summary %}
Updates a Belt by ID
{% endapi-method-summary %}

{% api-method-description %}
Updates attributes of a belt.   
  
In order to update a belt, a complete JSON representation of the object needs to be provided. For a list of body parameters, see above POST /belts documentation. Empty fields will be set to default values as specified for POST /belts. The Keycloak user needs to have the _editor_ roles defined for this Belt.  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
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
     "version": "2",
     "name": "hello-belt",
     "kubernetesName": "grnry-belt-hello-belt",
     "description": "Hello Belt Belt",
     "labels": [],
     "affectedPaths": [],
     "replicas": 1,
     "millicpu": 200,
     "memory": 512,
     "author": "User",
     "reader": [
       "\"_auth\""
     ],
     "editor": [
       "belt_edit"
     ],
     "viewer": [
       "belt_view"
     ],
     "created": 1562744768164,
     "assumedRole": "",
     "requirementsPy": "package1==0.0.0\r\npackage2",
     "extractorVersion": "0.5.0",
     "extractorFn": "from time import time\r\nfrom grnry.beltextractor.update import Update\r\n\r\ndef execute(headers, event, profile=None):\r\n print(profile)\r\n update = Update(headers['grnry-correlation-id'],[\"dummy\"]).set_value(\"Hallo Belt!\",0.5,time(),'P1D','Dummy-Belt')\r\n update.set_type('TestProfileType')\r\n return [update]\r\n",
     "eventTypes": [
       "test-a",
       "test-b"
     ],
     "partitionOffsets": {},
     "kafkaDestinationTopic": "profile-update",
     "beltType": "",
     "runtime": "",
     "parameter": "",
     "debug": false,
     "fetchProfile": "FALSE",
     "profileType": "test-type",
     "secret": "",
     "secretUsername": "",
     "secretPassword": "",
     "status": "STOPPED",
     "volumes": null,
     "volumeMounts": null,
     "extraEnv": null,
     "id": "161"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=304 %}
{% api-method-response-example-description %}
The provided body parameters did not override existing values.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If the belt structure contains an invalid event type. 
{% endapi-method-response-example-description %}

```
{
    "timestamp": "2019-11-01T15:45:29.385+0000",
    "message": " : Invalid event type: someUnknownEventType",
    "details": "uri=/belts/425"
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

{% api-method method="get" host="https://api.grnry.io" path="/belts/:id/state" %}
{% api-method-summary %}
Get a Belt's state
{% endapi-method-summary %}

{% api-method-description %}
Retrieve the status of the Belt's Kubernetes deployment.  
  
In order to get results, you must have the required roles as defined in the fields _editor_ or _viewer_. Otherwise, you will not get back any results.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
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
{ "status": "RUNNING"} or

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
                    "image": "docker-registry:5000/grnry/belt-extractor:extractorVersion",
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

Possible values for the status attribute in the response body are:

| Status | Description |
| :--- | :--- |
| RUNNING | Belt is running |
| FAILED | Belt is deployed but not running |
| RUNNING\_BUT\_OUTDATED | Belt is running but there is a newer version of it in the database |
| DEPLOYING | Belt is being deployed |
| STOPPED | Belt is not deployed |

{% api-method method="post" host="https://api.grnry.io" path="/belts/:id/state" %}
{% api-method-summary %}
Manipulate a Belt's state
{% endapi-method-summary %}

{% api-method-description %}
Update the status of the Belt's Kubernetes deployment. The Keycloak user needs to have the _editor_ roles defined for this Belt.  
  
Body example:  
`{"action": "START"}`  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
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
{    "status": "DEPLOYING"}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
If user is not privileged to manipulate this belt's state.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
If there's no belt associated with given belt ID.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

