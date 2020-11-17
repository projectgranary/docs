---
description: Harvester API's instance endpoints.
---

# Harvester Instance Endpoints

## Paths

* GET /harvesters/instances
* GET /harvesters/instances/{harvester-name}
* POST /harvesters/instances
* POST /harvesters/instances/{harvester-name}
* PUT /harvesters/instances/{harvester-name}
* DELETE /harvesters/instances/{harvester-name}
* GET /harvesters/instances/{harvester-name}/state
* POST /harvesters/instances/{harvester-name}/state

## API Methods

Consult the [Granary Access Clients Reference](../../../operator-reference/identity-and-access-management/granary-access-clients.md#harvester-api) for roles a user needs to interact with Harvester API.

{% api-method method="get" host="https://api.grnry.io" path="/harvesters/instances" %}
{% api-method-summary %}
Get all Harvester Instances
{% endapi-method-summary %}

{% api-method-description %}
returns a pageable list of all harvesters. Requires role `harvester_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="pagesize" type="integer" required=false %}
Number of harvesters returned, default is 20. Maximum is 250.
{% endapi-method-parameter %}

{% api-method-parameter name="expand" type="string" required=false %}
Include all harvesters' states with `expand=state` in response body.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="integer" required=false %}
Start offset. Default: 0. Must be a whole multiple of `pagesize`.
{% endapi-method-parameter %}

{% api-method-parameter name="search" type="string" required=false %}
Filter harvester list by name. Default: ""
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
   "harvesters":[
      {
         "name":"harvester-1",
         "_links":{
            "self":{
               "href":"https://hostname/harvesters/instances/harvester-1"
            }
         }
      },
      {
         "name":"demo-harvester",
         "_links":{
            "self":{
               "href":"https://hostname/harvesters/instances/demo-harvester"
            }
         }
      }
   ],
   "totalCount":2,
   "_links":{
      "self":{
         "href":"https://hostname/harvesters/instances?search=&offset=0&pagesize=20&expand=totalCount"
      }
   }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid query parameter value.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'pagesize' must be a natural number but is '-1'.",
    "details": "uri=/harvesters/instances"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/instances"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Missing roles to access this resource.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/harvesters/instances"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/harvesters/instances/:harvester-name" %}
{% api-method-summary %}
Get Harvester details
{% endapi-method-summary %}

{% api-method-description %}
Returns all details of a given harvester. Requires role `harvester_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="harvester-name" type="string" required=true %}
technical name of harvester
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="export" type="string" required=false %}
if set to `"true"` \(not case sensitive\), the harvester response will only contain properties that a POST body must necessarily contain to create this exact harvester. All properties set to default values and all api-generated properties will be omitted. \(Only the api-generated `name` field will be provided\). Default is `""`.
{% endapi-method-parameter %}

{% api-method-parameter name="expand" type="string" required=false %}
Show Harvester state with expand=state.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Sample Response
{% endapi-method-response-example-description %}

```text
{
    "name": "harvester-demo",
    "displayName": "Harvester Demo",
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
                "href": "https://hostname/harvesters/source-types/grnry-jdbc/latest"
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
                "href": "https://hostname/harvesters/instances/harvester-demo/state"
            }
        }
    },
    "_links": {
        "self": {
            "href": "https://hostname/harvesters/instances/harvester-demo"
        }
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/instances/adobe-s3-std"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Missing roles to access this resource.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/harvesters/instances/adobe-s3-std"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Harvester instance with given name \(case sensisitive\) does not exist.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Harvester 'harvester-delete' not found.",
    "details":"uri=/harvesters/instances/harvester-delete"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.grnry.io" path="/harvesters/instances/:harvester-name" %}
{% api-method-summary %}
Create Harvester
{% endapi-method-summary %}

{% api-method-description %}
Creates a new harvester instance. A technical name for the harvester instance will be derived from given `displayName` by removing all special characters, replacing white spaces with hyphens and limiting the length to 20 characters. Should the created harvester name be already in use the last four characters will be replaced by a suffix of numbers.  
  
Source type and event type are referenced by `name` and `version` in event\_types and source\_types tables. If no configuration properties are set under `sourceType` \(resp. `eventType`\) configuration from these entities will be applied.  
  
Requires the role `harvester_edit`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="harvester-name" type="string" required=false %}
unique technical harvester name
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="imported" type="string" required=false %}
if set to `"true"` \(not case sensitive\), the harvester default values will be merged into the provided POST body while keeping the custom values if provided.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="sessionizing" type="object" required=false %}
Sessionizing application used by this harvster. Contains flag `enabled:boolean` \(if set to false, no other fields should be set\). Default values for all other fields can be specified during harvester api deployment. These _optional_ fields are `app:string` \(registered app in scdf\), `version:string` \(app version registered in scdf\), `deploymentConfiguration:map`, `appConfiguration:map`, `correlationIdExpression:string`, `sessionIdExpression:string`, `inactivityGapSec:long`, `gracePeriodSec:long`.
{% endapi-method-parameter %}

{% api-method-parameter name="metadataExtractor" type="object" required=false %}
Metadata extractor application used by this harvester. If not specified, a default metadata extractor app will be deployed. Default values for all fields can be specified during harvester api deployment. _Optional_ fields are `app:string` \(registered app in scdf\), `version:string` \(registered app version in scdf\), `deploymentConfiguration:map`, `appConfiguration:map`.
{% endapi-method-parameter %}

{% api-method-parameter name="transform" type="object" required=false %}
Scriptable transform application used by this harvester. If not specified, a default transform app will be deployed. Default values for all fields can be specified during harvester api deployment. _Optional_ fileds are `app:string` \(registered app in scdf\), `version:string` \(app version registered in scdf\), `deploymentConfiguration:map`, `appConfiguration:map`, `language:string` \(script language\), `script:string` \(script that transforms the data\).
{% endapi-method-parameter %}

{% api-method-parameter name="sourceType" type="object" required=true %}
Existing sourceType that this harvester receives data from. _Required_ fields are `name:string` and `version:string`. _Optional_ fields are `configuration:map`, `deployerConfiguration:map`, `appConfiguration:map`
{% endapi-method-parameter %}

{% api-method-parameter name="eventType" type="object" required=true %}
Existing eventType that this harvester should process. _Required_ fields are `name:string` and `version:string`.
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
Harvester description
{% endapi-method-parameter %}

{% api-method-parameter name="displayName" type="string" required=true %}
Human readable name. Needs to be unique. A technical name will be derived from it.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "name": "harvester-post",
    "displayName": "Harvester Post",
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
            "href": "https://hostname/harvesters/instances/harvester-post"
        }
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Missing field or bad value
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'eventTyp.version' must not be null.",
    "details": "uri=/harvesters/instances"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/instances"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Missing roles to access this resource.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/harvesters/instances"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
harvester name \(case insesnsitive\) already exists
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1579697613983,
    "type": "entity_already_exists",
    "message": "Harvester 'harvester-post' already exists.",
    "details": "uri=/harvesters/instances"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="put" host="https://api.grnry.io/" path="harvesters/instances/:harvester-name" %}
{% api-method-summary %}
Update Harvester Instance
{% endapi-method-summary %}

{% api-method-description %}
Updates a harvester instance. All body parameters are optional. Harvester name field `name` is not changeable and will be ignored if provided. Empty fields will be set to `""`, missing fields will remain unchanged. It is not possible to replace apps \(`sourceType`, `metadataExtractor`, `transform`\), only their versions and configs are modifiable.  
  
Requires the role `harvester_edit`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="harvester-name" type="string" required=true %}
name of the harvester that should be updated
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="expand" type="string" required=false %}
Show Harvester state with expand=state.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="sessionizing" type="object" required=false %}
sessionizing application used by this harvester. _Optional_ fields \(can only be set if `sessionizing.enabled=true`\) are `version:string`, `deploymentConfiguration:map`, `appConfiguration:map`, `correlationIdExpression:string`, `sessioningAttributeExpression:string`, `inactivityGapSec:long`, `gracePeriodSec:long`
{% endapi-method-parameter %}

{% api-method-parameter name="metadataExtractor" type="object" required=false %}
metadata extractor application used by this harvester. Default values for all fields can be specified during harvester api deployment. _Optional_ fields are `version:string` \(registered app version in scdf\), `deploymentConfiguration:map`, `appConfiguration:map`.
{% endapi-method-parameter %}

{% api-method-parameter name="transform" type="object" required=false %}
scriptable transform application used by this harvester. Default values for all fields can be specified during harvester api deployment. _Optional_ fields are  `version:string` \(app version registered in scdf\), `deploymentConfiguration:map`, `appConfiguration:map`, `language:string` \(script language\), `script:string` \(script that transforms the data\).
{% endapi-method-parameter %}

{% api-method-parameter name="sourceType" type="object" required=false %}
existing sourceType that this harvester receives data from. _Optional_ fields are `version:string`, `configuration:map`, `deployerConfiguration:map`, `appConfiguration:map`
{% endapi-method-parameter %}

{% api-method-parameter name="eventType" type="object" required=false %}
existing eventType that this harvester should process. Changeable field `version:string`.
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
harvester description
{% endapi-method-parameter %}

{% api-method-parameter name="displayName" type="string" required=false %}
human readable name. Needs to be unique. Technical name of the harvester will remain unchanged.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "name": "harvester-post",
    "displayName": "Harvester Post",
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
                "href": "https://hostname/harvesters/instances/harvester-demo/state"
            }
        }
    },
    "_links": {
        "self": {
            "href": "https://hostname/harvesters/instances/harvester-post"
        }
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Missing parameter or parameter is invalid.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'displayName' length must be between 3 and 60 characters but is 101 characters.",
    "details": "uri=/harvesters/instances/demo-set-all"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/instances/adobe-s3-std"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Missing roles to access this resource.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/harvesters/instances/demo-set-all"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Harvester with provided name \(case sensitive\) not found
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Harvester 'demo-set-al' not found.",
    "details": "uri=/harvesters/instances/demo-set-al"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
Another harvester with provided `displayName` is already present.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1580290695918,
    "type": "entity_already_exists",
    "message": "Harvester with displayName 'Harvester Post' already exists.",
    "details": "uri=/harvesters/instances/demo-set-all"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host="https://api.grnry.io" path="/harvesters/instances/:harvester-name" %}
{% api-method-summary %}
Delete a Harvester Instance
{% endapi-method-summary %}

{% api-method-description %}
Deletes the given Harvester. If it is still running, it will automatically stopped before deletion.  
This request requires the role `harvester_edit`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="harvester-name" type="string" required=true %}
Technical name of the Harvester.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/instances/adobe-s3-std"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Missing roles to access this resource.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/harvesters/instances/adobe-s3-std"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/harvesters/instances/:harvester-name/state" %}
{% api-method-summary %}
Get Harvester Instance State
{% endapi-method-summary %}

{% api-method-description %}
Get the current state of a harvester instance.  
This request required the role `harvester_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="harvester-name" type="string" required=true %}
Technical name of the harvester.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "status": "RUNNING",
    "_links": {
        "self": {
            "href": "https://hostname/harvesters/instances/harvester-1/state"
        }
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/instances/adobe-s3-std/state"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Missing roles to access this resource.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/harvesters/instances/adobe-s3-std/state"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Harvester with provided name \(case sensitive\) not found.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Harvester 'adobe-s3-stdd' not found.",
    "details": "uri=/harvesters/instances/adobe-s3-stdd/state"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



Possible values for the status attribute in the response body are:

| Status | Description |
| :--- | :--- |
| UNKNOWN | status could not be determined |
| STOPPED | harvester is not deployed |
| DEPLOYING | harvester is being deployed |
| STOPPING | harvester is being stopped |
| RUNNING | harvester is running |
| RUNNING\_BUT\_OUTDATED | harvester is running but there is a newer version of it in the database |
| FAILED | harvester is deployed but not running |

{% api-method method="post" host="https://api.grnry.io" path="/harvesters/instances/:harvester-name/state" %}
{% api-method-summary %}
Start/Stop Harvester Instance
{% endapi-method-summary %}

{% api-method-description %}
Start or stop the state of the given Harvester.  
This request requires the roles  `harvester_edit`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="harvester-name" type="string" required=true %}
technical name of the Harvester.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="action" type="string" required=false %}
updates the status of this harvester. Possible values: `START`, `STOP`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "status": "DEPLOYING",
    "_links": {
        "self": {
            "href": "https://hostname/harvesters/instances/harvester-1/state"
        }
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/instances/adobe-s3-std"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Missing roles to access this resource.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/harvesters/instances/adobe-s3-std/state"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Harvester not found.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1580291007569,
    "type": "entity_not_found",
    "message": "Harvester 'demo-set-all' not found.",
    "details": "uri=/harvesters/instances/adobe-s3-stdd/state"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Failed to start.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586952826375,
    "type": "unexpected_error",
    "message": "An unexpected error occurred.",
    "details":"uri=/harvesters/instances/fail-on-start/state"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/harvesters/instances/:harvester-name/logs/:step-name" %}
{% api-method-summary %}
Get Harvester Instance Logs
{% endapi-method-summary %}

{% api-method-description %}
Get the logs of a specific Step of a specific Harvester Instance.  
This request requires the role `harvester_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="line" type="string" required=false %}
Number of maximum lines the log should contain. Default is `500`
{% endapi-method-parameter %}

{% api-method-parameter name="step-name" type="string" required=true %}
Name of the Harvester Step.  The name must be `sourceType`, `transform`, or `metadataExtractor`.
{% endapi-method-parameter %}

{% api-method-parameter name="harvester-name" type="string" required=true %}
Technical name of the Harvester.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="lines" type="integer" required=false %}
The last x lines of the log \(if available\).  
Value range: `1 .. 500`. Default: `500`.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

