---
description: Springboot-based microservice to manage belts registered in the Belt Store.
---

# Belt API

## Paths

* GET /projects/{project-name}/belts
* GET /projects/{project-name}/belts/{id}
* POST /projects/{project-name}/belts
* POST /projects/{project-name}/belts/{id}
* DELETE /projects/{project-name}/belts/{id}
* PUT /projects/{project-name}/belts/{id}
* GET/projects/{project-name}/{id}/state
* POST/projects/{project-name}/belts/{id}/state
* GET /projects/{project-name}/belts/{id}/logs

## API Methods

Consult the [Granary Access Clients Reference](../../operator-reference/identity-and-access-management/granary-access-clients.md#belt-api) for Keycloak roles a user needs to interact with Belt API.

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/belts" method="get" summary="Get all Belts" %}
{% swagger-description %}
Retrieves full dump of 

**all**

 belts of a project in the Belt Store as a list.

\




\


In order to get results, you must have one of the project's roles 

_editor _

or 

_viewer_

. Otherwise, you will not get back any results.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project. 

\




_Backwards compatibility:_

\


__

If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token
{% endswagger-parameter %}

{% swagger-parameter in="query" name="search" type="string" %}
Filter belts by name. Belt names need to be url encoded. Default "".
{% endswagger-parameter %}

{% swagger-parameter in="query" name="expand" type="array" %}
Array of belt states. For possible values, see table at GET belt state definition below.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="pagesize" type="string" %}
Number of belts to be returned. Default is 

`20`

. Maximum is 

`250`

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="string" %}
Start offset. Default is 

`0`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="A list of all belts along with their attributes and total count of belts stored in the Belt Store." %}
```
{
    "totalCount": 502,
    "_links": {
        "next": {
            "href": "https://hostname/projects/global/belts?offset=2&pagesize=2&expand="
        },
        "self": {
            "href": "https://hostname/projects/global/belts?offset=0&pagesize=2&expand="
        }
    },
    "items": [
        {
            "version": "1",
            "name": "kube_test_4",
            "projectName": "global",
            "kubernetesName": "grnry-belt-1011",
            "description": "",
            "labels": [
                "a"
            ],
            "affectedPaths": [],
            "replicas": 1,
            "millicpu": 200,
            "millicpuRequests": 100,
            "memory": 512,
            "memoryRequests": 256,
            "createdBy": {
                "name": "Arthur Author",
                "email": "arthur@author.com"
            },
            "createdAt": "2021-09-24T08:38:50.848Z",
            "modifiedBy": {
                "name": "Arthur Author",
                "email": "arthur@author.com"
            },
            "modifiedAt": "2021-09-24T08:38:50.848Z",
            "assumedRole": "",
            "requirementsPy": "",
            "image": "grnry-belt",
            "imagePullSecret": "grnry-pull-secret",
            "extractorVersion": "0.8.0",
            "extractorFn": "from time import time\r\nfrom grnry_belt.models.update import Update\r\n\r\ndef execute(event, profile=None):\r\n    print(profile)\r\n    update = Update(profile['correlationId'],[\"dummy\"]).set_value(\"Hallo Belt!\",0.5,time(),'P1D','Dummy-Belt')\r\n    update.set_type('TestProfileType')\r\n    return [update]\r\n",
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
            "kafkaConsumerGroupName": "grnry-belt-1011-1561974412801",
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
                    "href": "https://hostname/projects/global/belts/1011"
                }
            },
            "id": "1011"
        },
        {
            "version": "1",
            "name": "kube_test_7",
            "projectName": "global",
            "kubernetesName": "grnry-belt-1022",
            "description": "kube_description",
            "labels": [],
            "affectedPaths": [],
            "replicas": 1,
            "millicpu": 54,
            "millicpuRequests": 54,
            "memory": 1023,
            "memoryRequests": 1023,
            "createdBy": {
                "name": "Arthur Author",
                "email": "arthur@author.com"
            },
            "createdAt": "2021-09-24T08:38:50.848Z",
            "modifiedBy": {
                "name": "Arthur Author",
                "email": "arthur@author.com"
            },
            "modifiedAt": "2021-09-24T08:38:50.848Z",
            "assumedRole": "",
            "requirementsPy": "requirement==0.1.0",
            "image": "grnry-belt",
            "imagePullSecret": "grnry-pull-secret",            
            "extractorVersion": "latest",
            "extractorFn": "print('hallo welt')",
            "eventTypes": [
                "eventType1",
                "eventType2",
                "eventType3"
            ],
            "partitionOffsets": {},
            "kafkaDestinationTopic": "profile-update",
            "kafkaConsumerGroupName": "grnry-belt-1022-1561978858485",
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
                    "href": "https://hostname/projects/global/belts/1022"
                }
            },
            "id": "1022"
        }
    ]
}
```
{% endswagger-response %}

{% swagger-response status="400" description="" %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'pagesize' must be a natural number but is '-1'.",
    "details": "uri=/projects/global/belts"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/belts"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/belts"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/belts/:id" method="get" summary="Get a Specific Belt by ID" %}
{% swagger-description %}
Retrieves full dump of a specific belt with a specified ID.

\




\


In order to retrieve results here, it is necessary that you either have the project's 

_editor _

or 

_viewer _

role assigned to your user in keycloak.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the belt belongs to.

\




_Backwards compatibility:_

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="string" required="true" %}
The ID of belt
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication Token
{% endswagger-parameter %}

{% swagger-parameter in="query" name="export" type="string" %}
if set to 

`"true"`

 (not case sensitive), the belt response will only contain properties a POST body must necessarily contain to create this exact belt. All properties set to default values and all api-generated properties will be omitted. (Only the api-generated 

`id`

 field will be provided). Default is 

`""`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="JSON with attributes of belt with the specified ID." %}
```
{
    "version": "1",
    "name": "hello-belt",
    "projectName": "global",
    "kubernetesName": "grnry-belt-161",
    "description": "Hello Belt Belt",
    "labels": [],
    "affectedPaths": [],
    "replicas": 1,
    "millicpu": 200,
    "millicpuRequests": 200,
    "memory": 512,
    "memoryRequests": 512,
    "createdBy": {
        "name": "Arthur Author",
        "email": "arthur@author.com"
    },
    "createdAt": "2021-09-24T08:38:50.848Z",
    "modifiedBy": {
        "name": "Arthur Author",
        "email": "arthur@author.com"
    },
    "modifiedAt": "2021-09-24T08:38:50.848Z",
    "assumedRole": "",
    "requirementsPy": "package1==0.0.0\r\npackage2",
    "image": "grnry-belt",
    "imagePullSecret": "grnry-pull-secret",
    "extractorVersion": "0.8.0",
    "extractorFn": "from time import time\r\nfrom grnry_belt.models.update import Update\r\n\r\ndef execute(headers, event, profile=None):\r\n print(profile)\r\n update = Update(headers['grnry-correlation-id'],[\"dummy\"]).set_value(\"Hallo Belt!\",0.5,time(),'P1D','Dummy-Belt')\r\n update.set_type('TestProfileType')\r\n return [update]\r\n",
    "eventTypes": [
        "test-a",
        "test-b"
    ],
    "partitionOffsets": {},
    "kafkaDestinationTopic": "profile-update",
    "kafkaConsumerGroupName": "grnry-belt-161-1562744768164",
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
            "href": "https://hostname/projects/global/belts/161"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/belts/161"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/belts/425"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="When there is no belt found with the specified ID" %}
```
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Belt with ID '425' not found.",
    "details": "uri=/projects/global/belts/425"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/belts/:id" method="post" summary="Create and Store a Belt" %}
{% swagger-description %}
Creates and stores a belt into the Belt Store. As a response the whole belt configuration is returned. 

\




\


Requires the role 

`editor`

.

\




\




**JSON Schema Definitions**

\




\


Volume Mount: https://javadoc.io/doc/io.fabric8/kubernetes-model/3.0.1/io/fabric8/kubernetes/api/model/VolumeMount.html

\




\


Volume: https://javadoc.io/doc/io.fabric8/kubernetes-model/3.0.1/io/fabric8/kubernetes/api/model/Volume.html

\



{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project.

\




_Backwards compatibility:_

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" type="string" required="true" %}
Unique name of the belt.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="number" required="false" %}
If provided, creates a belt with a given belt 

`id`

. This 

`id`

 needs to be unique. Otherwise an 

`id`

 is automatically assigned. Value range for 

`id`

 is a positive 

`Long`

 value (i.e. 

`1`

 to 

`9223372036854775807`

).
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="string" %}
Belt description.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="eventTypes" type="array" required="true" %}
String array of event types to be processed. Only event types registered with Harvester API's event type endpoint are valid values. You must have the event type's 

_consumer_

 or 

_editor_

 role assigned in Keycloak to consume an event type in a belt.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="outputTypes" type="array" %}
Array of event type objects with keys 

`name`

 and 

`version`

. Mutually exclusive with 

`kafkaDestinationTopic`

. If not set, default of 

`kafkaDestinationTopic`

 is taken.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="kafkaDestinationTopic" type="string" %}
**[DEPRECATED]**

 Provide a different destination topic for this belt as the default. Defaults to Belt API Server setting for destination topic. Defaults to either 

`profile-update`

 or server env variable 

`BELT_DESTINATION_TOPIC`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="extractorVersion" type="string" %}
Image tag of the belt runtime docker image to be used. Defaults to either 

`latest`

 or server env variable 

`BELT_EXTRACTOR_VERSION`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="extractorFn" type="string" %}
Extractor function to be executed by this belt.

\


Defaults either to 

`Hello World`

 example function or server env variable 

`BELT_EXTRACTOR_FN`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="requirementsPy" type="string" %}
PIP package requirements for belt. E.g. 

`package1==0.1.0\r\npackage2`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="imagePullSecret" type="string" %}
Kubernetes Secret used to pull the 

`image`

 . Defaults to server env 

`BELT_PULL_SECRET`

 .
{% endswagger-parameter %}

{% swagger-parameter in="body" name="image" type="string" %}
Docker Image to be deployed. Defaults to server env 

`BELT_IMAGE_NAME`

 .
{% endswagger-parameter %}

{% swagger-parameter in="body" name="millicpu" type="string" %}
CPU limit specification for this belt. Defaults to either 

`200`

 or server env variable 

`BELT_MILLI_CPU`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="millicpuRequests" type="string" %}
CPU requests specification for this belt. Defaults to either 

`200`

 or server env variable 

`BELT_MILLI_CPU_REQUESTS`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="memory" type="string" %}
Memory limit specification for this belt. Defaults either to 

`512`

 or server env variable 

`BELT_MEMORY`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="memoryRequests" type="string" %}
Memory specification for this belt. Defaults to either 

`512`

 or server env variable 

`BELT_MEMORY_REQUESTS`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="replicas" type="integer" %}
Number of replicas. Defaults to 

`1`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="extraEnv" type="object" %}
Additional environment variables for Kubernetes belt deployment. Defaults to either 

`null`

 or server env variable 

`BELT_EXTRA_ENV`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="volumeMounts" type="object" %}
JSON Kubernetes volume mount definition for belt deployment. Defaults to either 

`null`

 or server env variable 

`BELT_VOLUME_MOUNTS`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="volumes" type="object" %}
JSON Kubernetes volume definition for belt deployment. Defaults to either 

`null`

 or server env variable 

`BELT_VOLUMES`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="partitionOffsets" type="object" %}
Mapping from input topics to respective start offsets which are provided as an array with the indices corresponding to the partition numbers. The offset settings only have an effect on the first start of a Belt. All subsequent (re-)starts of configuration update will read the event type topics from consumer group's current offset.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="debug" type="boolean" %}
Determines if belt should be run in debug mode. Defaults to 

`false`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="fetchProfile" type="string" %}
Determines if belt should fetch profiles from the Profile Store. Defaults to 

`FALSE`

. Possible values: 

`["FALSE", "TRUE", "LAZY"]`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="profileType" type="string" %}
Profile type to fetch. Defaults to 

`_d`

. 

**Required when fetchProfile is `TRUE`.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="secret" type="string" %}
Kubernetes Secret name used by belt to read values from Profile Store. 

**Required when fetchProfile is `TRUE`or `LAZY`**

_**.**_
{% endswagger-parameter %}

{% swagger-parameter in="body" name="secretUsername" type="string" %}
Username property in secret used by belt to read values from Profile Store.  

**Required when fetchProfile is `TRUE` or `LAZY`.**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="secretPassword" type="string" %}
Password property in secret used by the belt to read values from Profile Store. 

**Required when fetchProfile is `TRUE` or `LAZY`.**
{% endswagger-parameter %}

{% swagger-response status="200" description="Returns a full dump of belt object created." %}
```
{
    "version": "1",
    "name": "hello-belt",
    "projectName": "global",
    "kubernetesName": "grnry-belt-161",
    "description": "Hello Belt Belt",
    "labels": [],
    "affectedPaths": [],
    "replicas": 1,
    "millicpu": 200,
    "memory": 512,
    "createdBy": {
        "name": "Arthur Author",
        "email": "arthur@author.com"
    },
    "createdAt": "2021-09-24T08:38:50.848Z",
    "modifiedBy": {
        "name": "Arthur Author",
        "email": "arthur@author.com"
    },
    "modifiedAt": "2021-09-24T08:38:50.848Z",
    "assumedRole": "",
    "requirementsPy": "package1==0.0.0\r\npackage2",
    "image": "grnry-belt",
    "imagePullSecret": "grnry-pull-secret",    
    "extractorVersion": "0.8.0",
    "extractorFn": "from time import time\r\nfrom grnry_belt.models.update import Update\r\n\r\ndef execute(headers, event, profile=None):\r\n print(profile)\r\n update = Update(headers['grnry-correlation-id'],[\"dummy\"]).set_value(\"Hallo Belt!\",0.5,time(),'P1D','Dummy-Belt')\r\n update.set_type('TestProfileType')\r\n return [update]\r\n",
    "eventTypes": [
        "test-a",
        "test-b"
    ],
    "partitionOffsets": {},
    "kafkaDestinationTopic": "profile-update",
    "kafkaConsumerGroupName": "grnry-belt-161-1562744768164",
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
    "id": "161",
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/belts/161"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="If a belt with a given name exists already in the Belt Store." %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'extractorVersion' must not be empty.",
    "details": "uri=/projects/global/belts"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/belts"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing role to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/belts"
}
```
{% endswagger-response %}

{% swagger-response status="500" description="Some internal server issue has occurred." %}
```
{
    "timestamp": "2020-10-14T13:06:25.951+0000",
    "message": "An unexpected error occurred.",
    "type": "unexpected_error",
    "details": "uri=/projects/global/belts/"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/belts/:id" method="delete" summary="Delete a Specific Belt" %}
{% swagger-description %}
Deletes a specific belt given the ID.

\




\


Deletes the belt definition in the database and all corresponding kubernetes resources if the belt is deployed.

\




\


The Keycloak user needs to have the project's 

_editor_

 role.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the belt belongs to.

\




_Backwards compatibility:_

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="string" required="true" %}
Belt ID
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication Token
{% endswagger-parameter %}

{% swagger-response status="200" description="If delete is successful, content=true. content=false when unsuccessful" %}
```
{ "content": true } or { "content": false }
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/belts"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/belts/425"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="No belt found with given ID" %}
```
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Belt with ID '425' not found.",
    "details": "uri=/projects/global/belts/425"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/belts/:id" method="put" summary="Updates a Belt by ID" %}
{% swagger-description %}
Updates attributes of a belt, given its ID. 

\




\


In order to update a belt, a complete JSON representation of the object needs to be provided. For a list of body parameters, see above POST /belts documentation. Empty fields will be set to default values as specified for POST /belts. The Keycloak user needs to have the project's 

_editor_

 role.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the belt belongs to.

\




_Backwards compatibility:_

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="string" required="true" %}
Belt ID
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication Token
{% endswagger-parameter %}

{% swagger-response status="200" description="A full dump of belt object recently modified" %}
```
{
     "version": "2",
     "name": "hello-belt",
     "projectName": "global",
     "kubernetesName": "grnry-belt-161",
     "description": "Hello Belt Belt",
     "labels": [],
     "affectedPaths": [],
     "replicas": 1,
     "millicpu": 200,
     "millicpuRequests": 200,
     "memory": 512,
     "memoryRequests": 512,
     "createdBy": {
         "name": "Arthur Author",
         "email": "arthur@author.com"
     },
     "createdAt": "2021-09-24T08:38:50.848Z",
     "modifiedBy": {
         "name": "Other User",
         "email": "other@user.com"
     },
     "modifiedAt": "2021-09-29T07:39:57.628Z",
     "assumedRole": "",
     "requirementsPy": "package1==0.0.0\r\npackage2",
     "image": "custom-grnry-belt",
     "imagePullSecret": "custom-pull-secret", 
     "extractorVersion": "0.8.0",
     "extractorFn": "from time import time\r\nfrom grnry_belt.models.update import Update\r\n\r\ndef execute(headers, event, profile=None):\r\n print(profile)\r\n update = Update(headers['grnry-correlation-id'],[\"dummy\"]).set_value(\"Hallo Belt!\",0.5,time(),'P1D','Dummy-Belt')\r\n update.set_type('TestProfileType')\r\n return [update]\r\n",
     "eventTypes": [
       "test-a",
       "test-b"
     ],
     "partitionOffsets": {},
     "kafkaDestinationTopic": "profile-update",
     "kafkaConsumerGroupName": "grnry-belt-161-1562744768164",
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
             "href": "https://hostname/projects/global/belts/161"
         }
     }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="If the belt structure contains an invalid event type. " %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'beltId' must be a natural number but is 'abcd'.",
    "details": "uri=/projects/global/belts/abcd"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/belts"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/belts/425"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Belt with the given ID not found" %}
```
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Belt with ID '425' not found.",
    "details": "uri=/projects/global/belts/425"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/belts/:id/state" method="get" summary="Get a Belt's state" %}
{% swagger-description %}
Retrieve the status of the Belt's Kubernetes deployment.

\




\


In order to get results, you must have one of the project's roles 

_editor _

or 

_viewer_

. Otherwise, you will not get back any results.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the belt belongs to.

\




_Backwards compatibility:_

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="string" required="true" %}
Belt IDn
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{ "status": "RUNNING" } or

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
            "message": "ReplicaSet \"grnry-belt-161-54f5c68c89\" has timed out progressing.",
            "reason": "ProgressDeadlineExceeded",
            "status": "False",
            "type": "Progressing"
        }
    ],
    "podStatus": [
        {
            "name": "grnry-belt-161-54f5c68c89-8xf9v",
            "podEvents": [
                {
                    "lastTransitionTime": "2019-07-01T11:02:38Z",
                    "status": "True",
                    "type": "Initialized"
                },
                {
                    "lastTransitionTime": "2019-07-01T11:02:38Z",
                    "message": "containers with unready status: [grnry-belt-161]",
                    "reason": "ContainersNotReady",
                    "status": "False",
                    "type": "Ready"
                },
                {
                    "lastTransitionTime": "2019-07-01T11:02:38Z",
                    "message": "containers with unready status: [grnry-belt-161]",
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
                    "name": "grnry-belt-161",
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
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/belts"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/belts/425"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Belt with given ID not found." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_found",
    "message": "Belt with ID '425' not found.",
    "details": "uri=/projects/global/belts/425"
}
```
{% endswagger-response %}
{% endswagger %}

Possible values for the status attribute in the response body are:

| Status                 | Description                                                        |
| ---------------------- | ------------------------------------------------------------------ |
| RUNNING                | Belt is running                                                    |
| FAILED                 | Belt is deployed but not running                                   |
| RUNNING\_BUT\_OUTDATED | Belt is running but there is a newer version of it in the database |
| DEPLOYING              | Belt is being deployed                                             |
| STOPPED                | Belt is not deployed                                               |

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/belts/:id/state" method="post" summary="Manipulate a Belt's state" %}
{% swagger-description %}
Update the status of the Belt's Kubernetes deployment. The Keycloak user needs to have the 

_editor_

 roles defined for the Belt.

\




\


Body example:

\




`{"action": "START"}`

\



{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the belt belongs to.

\




_Backwards compatibility:_

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="string" required="true" %}
Belt ID
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="action" type="string" required="true" %}
The action to be performed "START" or "STOP"
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```javascript
{    "status": "DEPLOYING"}
```
{% endswagger-response %}

{% swagger-response status="400" description="Action is not allowed while belt is in current state or invalid." %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'state' cannot be 'START' while Belt status is 'DEPLOYING'.",
    "details": "uri=/projects/global/belts/abcd/state"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/belts/425/state"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/belts/425/state"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Belt with given ID not found." %}
```
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Belt with ID '425' not found.",
    "details": "uri=/projects/global/belts/425/state"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/belts/:id/logs" method="get" summary="Get Belt's Pod Logs by Id" %}
{% swagger-description %}
Get the last n log lines from all pods of the belt with the given 

`id`

 where n is specified by the 

`lines`

 query parameter.

\




\


This request requires the project's 

_editor _

role.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the belt belongs to.

\




_Backwards compatibility:_

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="string" required="true" %}
Belt ID
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token
{% endswagger-parameter %}

{% swagger-parameter in="query" name="lines" type="string" %}
number of lines to be retrieved per pod. Must be greater than 

`0`

 and less than or equal to 

`500`

. Default is 

`500`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "totalCount": 3,
    "logs": {
        "grnry-belt-59-768f5ddf6c-52ch8": [
            "2020-07-03T11:13:42.4858863Z 2020-07-03 11:13:42.485 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:13:47.486403173Z 2020-07-03 11:13:47.485 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:13:52.48683279Z 2020-07-03 11:13:52.486 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:13:57.487381034Z 2020-07-03 11:13:57.486 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:14:02.488080286Z 2020-07-03 11:14:02.487 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s"
        ],
        "grnry-belt-59-768f5ddf6c-tjp9f": [
            "2020-07-03T11:13:41.034269651Z 2020-07-03 11:13:41.033 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:13:46.034808213Z 2020-07-03 11:13:46.034 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:13:51.035408253Z 2020-07-03 11:13:51.034 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:13:56.035918458Z 2020-07-03 11:13:56.035 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:14:01.036559273Z 2020-07-03 11:14:01.036 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s"
        ],
        "grnry-belt-59-768f5ddf6c-z2hz5": [
            "2020-07-03T11:13:42.903696675Z 2020-07-03 11:13:42.903 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:13:47.904404575Z 2020-07-03 11:13:47.903 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:13:52.904817175Z 2020-07-03 11:13:52.904 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:13:57.905187867Z 2020-07-03 11:13:57.904 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s",
            "2020-07-03T11:14:02.905797986Z 2020-07-03 11:14:02.905 DEBUG [grnry-belt-59,,,] 12 --- [     MainThread] consumer                                 : No new messages, waiting for: 5s"
        ]
    },
    "_links": {
        "self": {
            "href": "https://api.grnry.io/projects/global/belts/59/logs?lines=5"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="" %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'lines' must be in between 1 and 500 but is '750'.",
    "details": "uri=/projects/global/belts/abcd/logs"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/belts/425/logs"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/belts/425/logs"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Belt with given ID not found." %}
```
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Belt with ID '425' not found.",
    "details": "uri=/projects/global/belts/425/logs"
}
```
{% endswagger-response %}
{% endswagger %}
