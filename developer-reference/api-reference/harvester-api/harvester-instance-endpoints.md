---
description: Harvester API's instance endpoints.
---

# Harvester Instance Endpoints

## Paths

* GET /projects/{project-name}/harvesters/instances
* GET /projects/{project-name}/harvesters/instances/{harvester-name}
* POST /projects/{project-name}/harvesters/instances
* POST /projects/{project-name}/harvesters/instances/{harvester-name}
* PUT /projects/{project-name}/harvesters/instances/{harvester-name}
* DELETE /projects/{project-name}/harvesters/instances/{harvester-name}
* GET /projects/{project-name}/harvesters/instances/{harvester-name}/state
* POST /projects/{project-name}/harvesters/instances/{harvester-name}/state

## API Methods

Consult the [Granary Access Clients Reference](../../../operator-reference/identity-and-access-management/granary-access-clients.md#harvester-api) for roles a user needs to interact with Harvester API.

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/harvesters/instances" method="get" summary="Get all Harvester Instances" %}
{% swagger-description %}
returns a pageable list of all harvesters. Requires role 

`viewer`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project. Retrieve all harvesters from this project scope.

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="pagesize" type="integer" %}
Number of harvesters returned, default is 20. Maximum is 250.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="expand" type="string" %}
Include all harvesters' states with 

`expand=state`

 in response body.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="integer" %}
Start offset. Default: 0. Must be a whole multiple of 

`pagesize`

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="search" type="string" %}
Filter harvester list by name. Default: ""
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" required="true" %}

{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
   "harvesters":[
      {
         "name":"harvester-1",
         
         ...
         
         "_links":{
            "self":{
               "href":"https://hostname/projects/global/harvesters/instances/harvester-1"
            }
         }
      },
      {
         "name":"demo-harvester",
         
         ...
         
         "_links":{
            "self":{
               "href":"https://hostname/projects/global/harvesters/instances/demo-harvester"
            }
         }
      }
   ],
   "totalCount":2,
   "_links":{
      "self":{
         "href":"https://hostname/projects/global/harvesters/instances?search=&offset=0&pagesize=20&expand=totalCount"
      }
   }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Invalid query parameter value." %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'pagesize' must be a natural number but is '-1'.",
    "details": "uri=/projects/global/harvesters/instances"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/harvesters/instances"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/harvesters/instances"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/harvesters/instances/:harvester-name" method="get" summary="Get Harvester details" %}
{% swagger-description %}
Returns all details of a given harvester. Requires role 

`viewer`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the harvester belongs to. 

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

{% swagger-parameter in="path" name="harvester-name" type="string" required="true" %}
technical name of harvester
{% endswagger-parameter %}

{% swagger-parameter in="query" name="export" type="string" %}
if set to 

`"true"`

 (not case sensitive), the harvester response will only contain properties that a POST body must necessarily contain to create this exact harvester. All properties set to default values and all api-generated properties will be omitted. (Only the api-generated 

`name`

 field will be provided). Default is 

`""`

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="expand" type="string" %}
Show Harvester state with expand=state.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" required="true" %}

{% endswagger-parameter %}

{% swagger-response status="200" description="Sample Response" %}
```
{
    "name": "harvester-demo",
    "displayName": "Harvester Demo",    
    "createdAt": "2021-09-23T10:09:44.711Z",
    "createdBy": {
        "name": "user",
        "email": "user@grnry.io"
    },
    "modifiedAt": "2021-09-23T10:11:10.171Z",
    "modifiedBy": {
        "name": "other-user",
        "email": "other-user@grnry.io"
    },
    "projectName": "global",
    "streamName": "g-h-harvester-demo",
    "dlqTopic": "grnry_harvester_dlq_harvester-demo",
    "sourceType": {
        "name": "grnry-jdbc",
        "version": "2.0",
        "configuration": {},
        "deploymentConfiguration": {
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]",
            "kubernetes.limits.cpu": "400m",
            "kubernetes.requests.cpu": "400m",
            "kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }]",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.imagePullPolicy": "Always",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.livenessProbeDelay": "120",
            "kubernetes.readinessProbeDelay": "120"
        },
        "appConfiguration": {},
        "_links": {
            "type": {
                "href": "https://hostname/projects/global/harvesters/source-types/grnry-jdbc/latest"
            }
        }
    },
    "metadataExtractor": {
        "app": "grnry-data-in-metadata",
        "version": "latest",
        "deploymentConfiguration": {
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.livenessprobedelay": "120",
            "kubernetes.readinessprobedelay": "120"
        },
        "appConfiguration": {
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.replicationfactor": "3",
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true"
        }
    },
    "transform": {
        "app": "grnry-scriptable",
        "version": "latest",
        "deploymentConfiguration": {
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.livenessprobedelay": "120",
            "kubernetes.readinessprobedelay": "120"
        },
        "appConfiguration": {
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.replicationfactor": "3",
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true"
        },
        "language": "groovy",
        "script": "return new String(payload , 'UTF-8');"
    },
    "sessionizing": {
        "app": "grnry-sessionizing",
        "version": "latest",
        "deploymentConfiguration": {
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.livenessprobedelay": "120",
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_private.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName: 'grnry-pg-citus-secret' , defaultMode: '256' }}]"
        },
        "appConfiguration": {
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
            "spring.cloud.stream.kafka.binder.consumer.maxattempts": "1",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.replicationfactor": "3"
        },
        "correlationIdExpression": "headers['grnry-correlation-id']",
        "sessionizingAttributeExpression": "",
        "inactivityGapSec": 1800,
        "gracePeriodSec": 120,
        "enabled": true
    },
    "eventType": {
        "name": "snowplow-a",
        "version": "1"
    },
    "state": {
        "status": "RUNNING",
        "_links": {
            "self": {
                "href": "https://hostname/projects/global/harvesters/instances/harvester-demo/state"
            }
        }
    },
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/harvesters/instances/harvester-demo"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/harvesters/instances/adobe-s3-std"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/harvesters/instances/adobe-s3-std"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Harvester instance with given name (case sensisitive) does not exist." %}
```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Harvester 'harvester-delete' not found.",
    "details":"uri=/projects/global/harvesters/instances/harvester-delete"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/harvesters/instances/:harvester-name" method="post" summary="Create Harvester" %}
{% swagger-description %}
Creates a new harvester instance. A technical name for the harvester instance will be derived from given 

`displayName`

 by removing all special characters, replacing white spaces with hyphens and limiting the length to 20 characters. Should the created harvester name be already in use the last four characters will be replaced by a suffix of numbers.

\




\


Source type and event type are referenced by 

`name`

 and 

`version`

 in event_types and source_types tables. If no configuration properties are set under 

`sourceType `

(resp. 

`eventType`

) configuration from these entities will be applied.

\




\


Requires the role 

`editor`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the harvester belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="harvester-name" type="string" %}
unique technical harvester name
{% endswagger-parameter %}

{% swagger-parameter in="body" name="displayName" type="string" %}
Human readable name. Needs to be unique. A technical name will be derived from it.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="imported" type="string" %}
if set to 

`"true"`

 (not case sensitive), the harvester default values will be merged into the provided POST body while keeping the custom values if provided.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="string" %}
Harvester description
{% endswagger-parameter %}

{% swagger-parameter in="body" name="eventType" type="object" %}
Existing eventType that this harvester should process. 

_Required _

fields are 

`name:string`

 and 

`version:string`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sourceType" type="object" %}
Existing sourceType that this harvester receives data from. 

_Required _

fields are 

`name:string `

and 

`version:string`

. 

_Optional _

fields are 

`configuration:map`

, 

`deployerConfiguration:map`

, 

`appConfiguration:map`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="transform" type="object" %}
Scriptable transform application used by this harvester. If not specified, a default transform app will be deployed. Default values for all fields can be specified during harvester api deployment. 

_Optional _

fileds are 

`app:string`

 (registered app in scdf), 

`version:string`

 (app version registered in scdf), 

`deploymentConfiguration:map`

, 

`appConfiguration:map`

, 

`language:string`

 (script language), 

`script:string`

 (script that transforms the data).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sessionizing" type="object" %}
Sessionizing application used by this harvster. Contains flag 

`enabled:boolean`

 (if set to false, no other fields should be set). Default values for all other fields can be specified during harvester api deployment. These 

_optional _

fields are 

`app:string`

 (registered app in scdf), 

`version:string`

 (app version registered in scdf), 

`deploymentConfiguration:map`

, 

`appConfiguration:map`

, 

`correlationIdExpression:string`

, 

`sessionIdExpression:string`

, 

`inactivityGapSec:long`

, 

`gracePeriodSec:long`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="metadataExtractor" type="object" %}
Metadata extractor application used by this harvester. If not specified, a default metadata extractor app will be deployed. Default values for all fields can be specified during harvester api deployment. 

_Optional _

fields are 

`app:string`

 (registered app in scdf), 

`version:string `

(registered app version in scdf), 

`deploymentConfiguration:map`

, 

`appConfiguration:map`

.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" required="true" %}

{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "name": "harvester-post",
    "displayName": "Harvester Post",
    "createdAt": "2021-09-23T10:09:44.711Z",
    "createdBy": {
        "name": "user",
        "email": "user@grnry.io"
    },
    "modifiedAt": "2021-09-23T10:09:44.711Z",
    "modifiedBy": {
        "name": "user",
        "email": "user@grnry.io"
    },
    "projectName": "global",
    "streamName": "g-h-harvester-demo",
    "dlqTopic": "grnry_harvester_dlq_harvester-post",
    "sourceType": {
        "name": "grnry-jdbc",
        "version": "latest",
        "configuration": {
            "password" : "secret",
            "username" : "su",
            "hostname" : "beefy-db-host"
        },
        "deploymentConfiguration": {
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]",
            "kubernetes.limits.cpu": "400m",
            "kubernetes.requests.cpu": "400m",
            "kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }]",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.imagePullPolicy": "Always",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.livenessProbeDelay": "120",
            "kubernetes.readinessProbeDelay": "120"
        },
        "appConfiguration": {
        
        }
    },
    "metadataExtractor": {
        "app": "grnry-data-in-metadata",
        "version": "1",
        "deploymentConfiguration": {
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.livenessprobedelay": "120",
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
            "kubernetes.imagepullpolicy": "Always"
        },
        "appConfiguration": {
            "spring.cloud.stream.kafka.binder.replicationfactor": "3",
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6"
        }
    },
    "transform": {
        "app": "grnry-scriptable",
        "version": "2",
        "deploymentConfiguration": {
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.limits.cpu": "500m"
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.livenessprobedelay": "120"
        },
        "appConfiguration": {
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
            "spring.cloud.stream.kafka.binder.replicationfactor": "3",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true"
        },
        "language": "groovy",
        "script": "return new String(payload , 'UTF-8');"
    },
    "sessionizing": {
        "app": "grnry-sessionizing",
        "version": "latest",
        "deploymentConfiguration": {
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.livenessprobedelay": "120",
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_private.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName: 'grnry-pg-citus-secret' , defaultMode: '256' }}]"
        },
        "appConfiguration": {
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
            "spring.cloud.stream.kafka.binder.consumer.maxattempts": "1",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.replicationfactor": "3"
        },
        "correlationIdExpression": "headers['grnry-correlation-id']",
        "sessionizingAttributeExpression": "",
        "inactivityGapSec": 1800,
        "gracePeriodSec": 120,
        "enabled": true
    },
    "eventType": {
        "name": "snowplow-a",
        "version": "latest"
    },
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/harvesters/instances/harvester-post"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Missing field or bad value" %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'eventTyp.version' must not be null.",
    "details": "uri=/projects/global/harvesters/instances"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/harvesters/instances"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/harvesters/instances"
}
```
{% endswagger-response %}

{% swagger-response status="409" description="harvester name (case insesnsitive) already exists" %}
```
{
    "timestamp": 1579697613983,
    "type": "entity_already_exists",
    "message": "Harvester 'harvester-post' already exists.",
    "details": "uri=/projects/global/harvesters/instances"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/harvesters/instances/:harvester-name" method="put" summary="Update Harvester Instance" %}
{% swagger-description %}
Updates a harvester instance. All body parameters are optional. Harvester name field 

`name`

 is not changeable and will be ignored if provided. Empty fields will be set to 

`""`

, missing fields will remain unchanged. It is not possible to replace apps (

`sourceType`

, 

`metadataExtractor`

, 

`transform`

), only their versions and configs are modifiable.

\




\


Requires the role 

`editor`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the Harvester belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="harvester-name" type="string" required="true" %}
Name of the Harvester that should be updated
{% endswagger-parameter %}

{% swagger-parameter in="query" name="imported" type="string" %}
If set to 

`true`

 (not case sensitive), the Harvester default values will be merged into the provided PUT body while keeping the custom values if provided.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="expand" type="string" %}
Show Harvester state with expand=state.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="displayName" type="string" %}
Human readable name for UI. Needs to be unique. Technical name of the harvester will remain unchanged.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="string" %}
Harvester description.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="eventType" type="object" %}
Existing 

`data_in`

 event type that this Harvester should process. Changeable field 

`version:string`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sourceType" type="object" %}
Existing source type that this harvester receives data from. 

_Optional _

fields are 

`version:string`

, 

`configuration:map`

, 

`deployerConfiguration:map`

, 

`appConfiguration:map`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="transform" type="object" %}
Scriptable transform application used by this Harvester. Default values for all fields can be specified during harvester api deployment. 

_Optional _

fields are  

`version:string`

 (app version registered in scdf), 

`deploymentConfiguration:map`

, 

`appConfiguration:map`

, 

`language:string`

 (script language), 

`script:string`

 (script that transforms the data).
{% endswagger-parameter %}

{% swagger-parameter in="body" name="sessionizing" type="object" %}
Sessionizing application used by this Harvester. 

_Optional _

fields (can only be set if 

`sessionizing.enabled=true`

) are 

`version:string`

, 

`deploymentConfiguration:map`

, 

`appConfiguration:map`

, 

`correlationIdExpression:string`

, 

`sessioningAttributeExpression:string`

, 

`inactivityGapSec:long`

, 

`gracePeriodSec:long`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="metadataExtractor" type="object" %}
Metadata extractor application used by this Harvester. Default values for all fields can be specified during harvester api deployment. 

_Optional _

fields are 

`version:string`

 (registered app version in SCDF), 

`deploymentConfiguration:map`

, 

`appConfiguration:map`

.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" required="true" %}

{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "name": "harvester-post",
    "displayName": "Harvester Post",    
    "createdAt": "2021-09-23T10:09:44.711Z",
    "createdBy": {
        "name": "user",
        "email": "user@grnry.io"
    },
    "modifiedAt": "2021-09-23T10:11:10.171Z",
    "modifiedBy": {
        "name": "other-user",
        "email": "other-user@grnry.io"
    },
    "projectName": "global",
    "streamName": "g-h-harvester-demo",
    "dlqTopic": "grnry_harvester_dlq_harvester-post",
    "sourceType": {
        "name": "grnry-jdbc",
        "version": "latest",
        "configuration": {
            "password": "secret",
            "username": "su",
            "hostname": "beefy-db-host"
        },
        "deploymentConfiguration": {
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]",
            "kubernetes.limits.cpu": "400m",
            "kubernetes.requests.cpu": "400m",
            "kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }]",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.imagePullPolicy": "Always",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.livenessProbeDelay": "120",
            "kubernetes.readinessProbeDelay": "120"
        },
        "appConfiguration": {

        }
    },
    "metadataExtractor": {
        "app": "grnry-data-in-metadata",
        "version": "1",
        "deploymentConfiguration": {
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.livenessprobedelay": "120",
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
            "kubernetes.imagepullpolicy": "Always"
        },
        "appConfiguration": {
            "spring.cloud.stream.kafka.binder.replicationfactor": "3",
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6"
        }
    },
    "transform": {
        "app": "grnry-scriptable",
        "version": "2",
        "deploymentConfiguration": {
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.limits.cpu": "500m"
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.livenessprobedelay": "120"
        },
        "appConfiguration": {
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
            "spring.cloud.stream.kafka.binder.replicationfactor": "3",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true"
        },
        "language": "groovy",
        "script": "return new String(payload , 'UTF-8');"
    },
    "sessionizing": {
        "app": "grnry-sessionizing",
        "version": "latest",
        "deploymentConfiguration": {
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.livenessprobedelay": "120",
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_private.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName: 'grnry-pg-citus-secret' , defaultMode: '256' }}]"
        },
        "appConfiguration": {
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
            "spring.cloud.stream.kafka.binder.consumer.maxattempts": "1",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.replicationfactor": "3"
        },
        "correlationIdExpression": "headers['grnry-correlation-id']",
        "sessionizingAttributeExpression": "",
        "inactivityGapSec": 1800,
        "gracePeriodSec": 120,
        "enabled": true
    },
    "eventType": {
        "name": "snowplow-a",
        "version": "latest"
    },
    "state": {
        "status": "RUNNING_BUT_OUTDATED",
        "_links": {
            "self": {
                "href": "https://hostname/projects/global/harvesters/instances/harvester-demo/state"
            }
        }
    },
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/harvesters/instances/harvester-post"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Missing parameter or parameter is invalid." %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'displayName' length must be between 3 and 60 characters but is 101 characters.",
    "details": "uri=/projects/global/harvesters/instances/demo-set-all"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/harvesters/instances/adobe-s3-std"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/harvesters/instances/demo-set-all"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Harvester with provided name (case sensitive) not found" %}
```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Harvester 'demo-set-al' not found.",
    "details": "uri=/projects/global/harvesters/instances/demo-set-al"
}
```
{% endswagger-response %}

{% swagger-response status="409" description="Another harvester with provided displayName is already present." %}
```
{
    "timestamp": 1580290695918,
    "type": "entity_already_exists",
    "message": "Harvester with displayName 'Harvester Post' already exists.",
    "details": "uri=/projects/global/harvesters/instances/demo-set-all"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/harvesters/instances/:harvester-name" method="delete" summary="Delete a Harvester Instance" %}
{% swagger-description %}
Deletes the given Harvester. If it is still running, it will automatically stopped before deletion.

\


This request requires the role 

`editor`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the harvester belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="harvester-name" type="string" required="true" %}
Technical name of the Harvester.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-response status="204" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/harvesters/instances/adobe-s3-std"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/harvesters/instances/adobe-s3-std"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/harvesters/instances/:harvester-name/state" method="get" summary="Get Harvester Instance State" %}
{% swagger-description %}
Get the current state of a harvester instance.

\


This request required the role 

`viewer`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the harvester belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="harvester-name" type="string" required="true" %}
Technical name of the harvester.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "status": "RUNNING",
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/harvesters/instances/harvester-1/state"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/harvesters/instances/adobe-s3-std/state"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/harvesters/instances/adobe-s3-std/state"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Harvester with provided name (case sensitive) not found." %}
```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Harvester 'adobe-s3-stdd' not found.",
    "details": "uri=/projects/global/harvesters/instances/adobe-s3-stdd/state"
}
```
{% endswagger-response %}
{% endswagger %}



Possible values for the status attribute in the response body are:

| Status                 | Description                                                             |
| ---------------------- | ----------------------------------------------------------------------- |
| UNKNOWN                | status could not be determined                                          |
| STOPPED                | harvester is not deployed                                               |
| DEPLOYING              | harvester is being deployed                                             |
| STOPPING               | harvester is being stopped                                              |
| RUNNING                | harvester is running                                                    |
| RUNNING\_BUT\_OUTDATED | harvester is running but there is a newer version of it in the database |
| FAILED                 | harvester is deployed but not running                                   |

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/harvesters/instances/:harvester-name/state" method="post" summary="Start/Stop Harvester Instance" %}
{% swagger-description %}
Start or stop the state of the given Harvester.

\


This request requires the roles  

`harvester_edit`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the harvester belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="harvester-name" type="string" required="true" %}
technical name of the Harvester.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="action" type="string" required="true" %}
updates the status of this harvester. Possible values: 

`START`

, 

`STOP`
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "status": "DEPLOYING",
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/harvesters/instances/harvester-1/state"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/harvesters/instances/adobe-s3-std"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/projects/global/harvesters/instances/adobe-s3-std/state"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Harvester not found." %}
```
{
    "timestamp": 1580291007569,
    "type": "entity_not_found",
    "message": "Harvester 'demo-set-all' not found.",
    "details": "uri=/projects/global/harvesters/instances/adobe-s3-stdd/state"
}
```
{% endswagger-response %}

{% swagger-response status="500" description="Failed to start." %}
```
{
    "timestamp":1586952826375,
    "type": "unexpected_error",
    "message": "An unexpected error occurred.",
    "details":"uri=/projects/global/harvesters/instances/fail-on-start/state"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/harvesters/instances/:harvester-name/logs/:step-name" method="get" summary="Get Harvester Instance Logs" %}
{% swagger-description %}
Get the logs of a specific Step of a specific Harvester Instance.

\


This request requires the role 

`harvester_read`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the harvester belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="step-name" type="string" required="true" %}
Name of the Harvester Step.  The name must be 

`sourceType`

, 

`transform`

, or 

`metadataExtractor`

.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="harvester-name" type="string" required="true" %}
Technical name of the Harvester.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="lines" type="integer" %}
The last x lines of the log (if available).

\


Value range: 

`1 .. 500`

. Default: 

`500`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "logs": [
        "2021-08-16 10:00:25.807  INFO [g-h-harvester-src,,,] 1 --- [-write-health-1] o.a.k.c.c.internals.AbstractCoordinator  : [Consumer clientId=healthindicator-kafka25ae8853-5ed6-4713-8437-36e8d3e71024, groupId=healthindicator-kafka-97538b4c-ccad-452a-a367-1f7354112ff2] (Re-)joining group",
        "2021-08-16 10:01:25.807  INFO [g-h-harvester-src,,,] 1 --- [-write-health-1] o.a.k.c.c.internals.AbstractCoordinator  : [Consumer clientId=healthindicator-kafka25ae8853-5ed6-4713-8437-36e8d3e71024, groupId=healthindicator-kafka-97538b4c-ccad-452a-a367-1f7354112ff2] (Re-)joining group"
    ]
}
```
{% endswagger-response %}
{% endswagger %}
