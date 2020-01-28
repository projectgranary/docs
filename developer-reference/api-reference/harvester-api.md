---
description: Springboot-based microservice to manage harvesters and event types.
---

# Harvester API

## Paths

### Source Type Endpoints

* GET /source-types
* GET /source-types/{source-type-name}
* GET /source-types/{source-type-name}/{version}

### Harvester Instance Endpoints

* GET /harvesters/instances
* GET /harvesters/instances/{harvester-name}
* POST /harvesters/instances

### Event Type Endpoints

* GET /event-types
* GET /event-types/{event-type-name}
* POST /event-types
* PUT/event-types/{event-type-name}
* DELETE /event-types/{event-type-name} 
* GET/event-types/{event-type-name}/{version}
* GET /event-types/{event-type-name}/eventstores/{event-store-name}/persister
* PUT /event-types/{event-type-name}/eventstores/{event-store-name}/persister
* GET /event-types/{event-type-name}/eventstores/{event-store-name}/persister/state
* POST /event-types/{event-type-name}/eventstores/{event-store-name}/persister/state
* GET /event-types/{event-type-name}/eventstores/{event-store-name}/persister/logs

{% api-method method="get" host="https://api.grnry.io" path="/source-types" %}
{% api-method-summary %}
Get all Source Types
{% endapi-method-summary %}

{% api-method-description %}
Get latest version of all source-types.  
This request requires the role `source_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="expand" type="string" required=false %}
Expand the response with `totalCount` to show the count of different source-type-names. Default is `""`.
{% endapi-method-parameter %}

{% api-method-parameter name="search" type="string" required=false %}
Filter source types by names containing this search term Default is `""`.
{% endapi-method-parameter %}

{% api-method-parameter name="pagesize" type="string" required=false %}
Number of source types returned per page. Default is `20`.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="string" required=false %}
Offset of the requested page. Default is `0`.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "sourceTypes": [
        {
            "name": "grnry-adobe-sftp",
            "_links": {
                "self": {
                    "href": "https://hostname/harvesters/source-types/grnry-adobe-sftp?offset=0&pagesize=20&expand="
                }
            }
        },
        {
            "name": "grnry-http",
            "_links": {
                "self": {
                    "href": "https://hostname/harvesters/source-types/grnry-http?offset=0&pagesize=20&expand="
                }
            }
        },
        {
            "name": "grnry-jdbc",
            "_links": {
                "self": {
                    "href": "https://hostname/harvesters/source-types/grnry-jdbc?offset=0&pagesize=20&expand="
                }
            }
        },
        {
            "name": "grnry-sftp",
            "_links": {
                "self": {
                    "href": "https://hostname/harvesters/source-types/grnry-sftp?offset=0&pagesize=20&expand="
                }
            }
        }
    ],
    "totalCount": 4,
    "_links": {
        "self": {
            "href": "https://hostname/harvesters/source-types?search=&offset=0&pagesize=20&expand=totalCount"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/source-types/:source-type-name" %}
{% api-method-summary %}
Get all versions of an Source Type
{% endapi-method-summary %}

{% api-method-description %}
Get all versions of a given source type.  
This request requires the role `source_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="source-type-name" type="string" required=true %}
Name of the source type.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="expand" type="string" required=false %}
Expand the response with `totalCount` to show the count of different versions. Default is `""` .
{% endapi-method-parameter %}

{% api-method-parameter name="pagesize" type="string" required=false %}
Number of source types returned per page. Default is  
`20` .
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="string" required=false %}
Offset of the requested page. Default is `0` . 
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "sourceTypes": [
        {
            "name": "grnry-jdbc",
            "version": "latest",
            "description": "grnry-jdbc source",
            "metadataUri": "maven://org.springframework.cloud.stream.app:jdbc-source-kafka:jar:metadata:2.1.2.RELEASE",
            "uri": "docker://registry:5000/grnry/scdf-apps/grnry-jdbc-source:latest",
            "deployerConfig": {
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
            "_links": {
                "self": {
                    "href": "https://hostname/harvesters/source-types/grnry-jdbc/latest"
                }
            }
        }
    ],
    "_links": {
        "self": {
            "href": "https://hostname/harvesters/source-types?search=grnry-jdbc&offset=0&pagesize=20&expand="
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/source-types/:source-type-name/:version" %}
{% api-method-summary %}
Get a Specific Version of Source Type
{% endapi-method-summary %}

{% api-method-description %}
Get one version of a source type.  
This request requires the role `source_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="version" type="string" required=true %}
Version of the source type.
{% endapi-method-parameter %}

{% api-method-parameter name="source-type-name" type="string" required=true %}
Name of the source type.
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

```
{
    "name": "grnry-jdbc",
    "version": "latest",
    "description": "grnry-jdbc source",
    "metadataUri": "maven://org.springframework.cloud.stream.app:jdbc-source-kafka:jar:metadata:2.1.2.RELEASE",
    "uri": "docker://registry:5000/grnry/scdf-apps/grnry-jdbc-source:latest",
    "config" : [
        {
            "name" : "username",
            "defaultValue" : null,
            "description" : "the database user name",
            "type": ,
            "deprecated" : false,
            "hints" : [ .... ]
        },
        {
            "name" : "password",
            "defaultValue" : null,
            "description" : "the database password",
            "type": ,
            "deprecated" : false,
            "hints" : [ .... ]
        },
        ....
    ],
    "appConfig" : {
        "spring.cloud.stream.kafka.binder.replicationfactor": "3"
    },
    "deployerConfig": {
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
    "_links": {
        "self": {
            "href": "https://hostname/harvesters/source-types/grnry-jdbc/latest"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/event-types" %}
{% api-method-summary %}
Get all Event Types
{% endapi-method-summary %}

{% api-method-description %}
Get latest version of all event-types.  
This request requires the role `event_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="expand" type="string" required=false %}
Expand the response with `totalCount`, or `persisters` to show either the count or the peristers of different event-type-names. Default is `""`.
{% endapi-method-parameter %}

{% api-method-parameter name="search" type="string" required=false %}
Filter event types by names containing this search term Default is `""`.
{% endapi-method-parameter %}

{% api-method-parameter name="pagesize" type="string" required=false %}
Number of event types returned per page. Default is `20`.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="string" required=false %}
Offset of the requested page. Default is `0`.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "_links": {
        "self": {
            "href": "https://hostname/event-types?search=&offset=0&pagesize=20&expand="
        }
    },
    "eventTypes": [
        {
            "name": "event-type-3",
            "version": "1",
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/event-type-3/1"
                }
            }
        },
        {
            "name": "postman-event-type",
            "version": "4",
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/postman-event-type/4"
                }
            }
        },
        {
            "name": "postman-test-event-type",
            "version": "2",
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/postman-test-event-type/2"
                }
            }
        }
    ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/event-types/:event-type-name" %}
{% api-method-summary %}
Get all versions of an Event Type
{% endapi-method-summary %}

{% api-method-description %}
Get all versions of a given event type.   
This request requires the role `event_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="event-type-name" type="string" required=true %}
Name of the event type.
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
All versions of the requested `:event-type-name` \(`postman-event-type`\).
{% endapi-method-response-example-description %}

```
{
    "_links": {
        "self": {
            "href": "https://hostname/event-types/postman-event-type"
        }
    },
    "eventTypes": [
        {
            "name": "postman-event-type",
            "version": "1",
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/postman-event-type/1"
                }
            }
        },
        {
            "name": "postman-event-type",
            "version": "2",
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/postman-event-type/2"
                }
            }
        },
        {
            "name": "postman-event-type",
            "version": "3",
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/postman-event-type/3"
                }
            }
        },
        {
            "name": "postman-event-type",
            "version": "4",
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/postman-event-type/4"
                }
            }
        }
    ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.grnry.io" path="/event-types" %}
{% api-method-summary %}
Create an Event Type
{% endapi-method-summary %}

{% api-method-description %}
This request requires the role `event_type_edit`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="description" type="string" required=false %}
description of the eventType.
{% endapi-method-parameter %}

{% api-method-parameter name="eventIdExpression" type="string" required=true %}
The SpringEL expression to create the grnry-event-id.
{% endapi-method-parameter %}

{% api-method-parameter name="correlationIdExpression" type="string" required=true %}
The SpringEL expression to create the grnry-correlation-id.
{% endapi-method-parameter %}

{% api-method-parameter name="timestampExpression" type="string" required=true %}
The SpringEL Expression to create the grnry-event-timestamp \(Milliseconds since 1.1.1970 UTC\)
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
Name of the new event type
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "name": "event-type-4",
    "version": "1",
    "_links": {
        "self": {
            "href": "https://hostname/event-types/event-type-4/1"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="put" host="https://api.grnry.io" path="/event-types/:event-type-name" %}
{% api-method-summary %}
Update an Event Type
{% endapi-method-summary %}

{% api-method-description %}
Fully or partially updates an event type.  
This requires the requester to assume roles of both `event-type-read` and `event-type-edit`.   
  
If no delta is recognized, no update will be made and HTTP 304 will be returned.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="event-type-name" type="string" required=true %}
Name of the event type 
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

```
{
   "name":"test-event-type",
   "version":"latest",
   "correlationIdExpression":"$['new']['correlation']['id']",
   "eventIdExpression":"$['test']['path']['eventId']",
   "timestampExpression":"$['test']['path']['timestamp']",
   "_links":{
      "self":{
         "href":"http://hostname/event-types/test-event-type/latest"
      }
   }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=304 %}
{% api-method-response-example-description %}
If the event type is not modified.
{% endapi-method-response-example-description %}

```
{
   "reason":"No changes detected",
   "entity":{
      "name":"test-event-type",
      "version":2,
      "correlationIdExpression":"$['test']['path']['correlationId']",
      "eventIdExpression":"$['test']['path']['eventId']",
      "timestampExpression":"$['test']['path']['timestamp']",
      "links":[

      ]
   }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host="https://api.grnry.io" path="/event-types/:event-type-name" %}
{% api-method-summary %}
Delete an Event Type
{% endapi-method-summary %}

{% api-method-description %}
Deletes all versions of the given event type, if it is not used by registered belts. This request requires the roles `event_type_read` and `event_type_edit`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="event-type-name" type="string" required=false %}
name of the event type to be deleted
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=412 %}
{% api-method-response-example-description %}
There are registered belts registered, that read from the event type topic so it can't be deleted. The response provides links to all of them.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1574854606738,
    "message": "event type with name test-event-type-delete is still used by belts: 
    [https://development.analytics.ventures.syncier.cloud/belts/11,
     https://development.analytics.ventures.syncier.cloud/belts/12,
     https://development.analytics.ventures.syncier.cloud/belts/13,
     https://development.analytics.ventures.syncier.cloud/belts/14,
     https://development.analytics.ventures.syncier.cloud/belts/15,
     https://development.analytics.ventures.syncier.cloud/belts/16,
     https://development.analytics.ventures.syncier.cloud/belts/17,
     https://development.analytics.ventures.syncier.cloud/belts/18,
     https://development.analytics.ventures.syncier.cloud/belts/19]",
    "details": "uri=/event-types/test-event-type-delete"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/event-types/:event-type-name/:version" %}
{% api-method-summary %}
Get a Specific Version of an Event Type
{% endapi-method-summary %}

{% api-method-description %}
Get one version of an event type.  
Version should be "latest" or a valid number greater than or equals to 1.  
This request requires the role `event_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="version" type="string" required=true %}
The version of the event type, or "latest" to get the latest version
{% endapi-method-parameter %}

{% api-method-parameter name="event-type-name" type="string" required=true %}
Name of the event type
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

```
{
    "name": "event-type-1",
    "version": "4",
    "_links": {
        "self": {
            "href": "https://hostname/event-types/event-type-1/4"
        }
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If the version is not latest or a valid number greater or equals to 1.
{% endapi-method-response-example-description %}

```
{
   "timestamp":1578934021414,
   "message":"version : Needs to be a long or latest",
   "details":"uri=/event-types/test-event-type/invalidVersion"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/event-types/:event-type-name/eventstores/:event-store-name/persister" %}
{% api-method-summary %}
Get Persister for a Specific Event Type
{% endapi-method-summary %}

{% api-method-description %}
Get the persister of an event type.  
This request requires the role `event_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="event-store-name" type="string" required=true %}
Name of the event store.
{% endapi-method-parameter %}

{% api-method-parameter name="event-type-name" type="string" required=true %}
Name of the event type.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="expand" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
requested :event-type-name \(eventtype-1\) , :event-store-name \(pg\)
{% endapi-method-response-example-description %}

```
{
    "streamName": "grnry-p-pg-postman-eventtype-1",
    "appName": "grnry-eventstore-pg",
    "appVersion": "latest",
    "appConfig": {
        "eventstore.tablename": "public.eventstore",
        "spring.datasource.url": "jdbc:postgresql://host:5432/postgres?currentSchema=public",
        "spring.datasource.password": "${superuser-password}",
        "spring.datasource.username": "${superuser-username}",
        "spring.cloud.kubernetes.secrets.paths": "/usr/src/app/db-secret",
        "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
        "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
        "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
        "spring.cloud.stream.kafka.binder.replicationfactor": "3",
        "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
        "spring.cloud.stream.bindings.input.consumer.partitioned": "true"
    },
    "deployerConfig": {
        "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
        "kubernetes.limits.cpu": "500m",
        "kubernetes.requests.cpu": "500m",
        "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
        "kubernetes.limits.memory": "512Mi",
        "kubernetes.imagepullpolicy": "Always",
        "kubernetes.requests.memory": "512Mi",
        "kubernetes.livenessprobedelay": "120",
        "kubernetes.readinessprobedelay": "120"
    },
    "eventTypeName": "eventtype-1",
    "eventstoreType": "pg",
    "lastUpdated": 1574417165615,
    "_links": {
        "self": {
            "href": "https://hostname/event-types/eventtype-1/eventstores/pg/persister"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="put" host="https://api.grnry.io" path="/event-types/:event-type-name/eventstores/:event-store-name/persister" %}
{% api-method-summary %}
Update Persister Config for a Specific Event Type
{% endapi-method-summary %}

{% api-method-description %}
Updates the persister configuration of a specific event type.  
This request requires the role `event_type_edit` and `event_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="event-type-name" type="string" required=true %}
Name of the event type.
{% endapi-method-parameter %}

{% api-method-parameter name="event-store-name" type="string" required=true %}
Name of the event store.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="deployerConfig" type="object" required=false %}
A map containing all deployer configuration options.
{% endapi-method-parameter %}

{% api-method-parameter name="appConfig" type="object" required=false %}
A Map containing all application configuration parameters
{% endapi-method-parameter %}

{% api-method-parameter name="appVersion" type="string" required=false %}
The updated stream app version. This version has to be present in the backend spring cloud data flow server, otherwise the update will fail.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "streamName": "grnry-p-pg-postman-eventtype-1",
    "appName": "grnry-eventstore-pg",
    "appVersion": "latest",
    "appConfig": {
        "eventstore.tableName": "public.eventstore",
        "spring.datasource.url": "jdbc:postgresql://grnry-pg-citus:5432/postgres?currentSchema=public",
        "spring.datasource.password": "${superuser-password}",
        "spring.datasource.username": "${superuser-username}",
        "spring.cloud.kubernetes.secrets.paths": "/usr/src/app/db-secret",
        "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
        "spring.cloud.stream.bindings.input.consumer.partitioned": "true"
    },
    "deployerConfig": {
        "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
        "kubernetes.limits.cpu": "700m",
        "kubernetes.requests.cpu": "700m",
        "kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
        "kubernetes.limits.memory": "512Mi",
        "kubernetes.imagePullPolicy": "Always",
        "kubernetes.requests.memory": "512Mi",
        "kubernetes.livenessProbeDelay": "120",
        "kubernetes.readinessProbeDelay": "120"
    },
    "eventTypeName": "postman-eventtype-1",
    "eventstoreType": "pg",
    "lastUpdated": 1574417943266,
    "_links": {
        "self": {
            "href": "https://hostname/event-types/eventtype-1/eventstores/pg/persister?expand="
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/event-types/:event-type-name/eventstores/:event-store-name/persister/state" %}
{% api-method-summary %}
Get State of a Persister for a Specific Event Type
{% endapi-method-summary %}

{% api-method-description %}
Get the current state of a persister for an event type.  
This request requires the role `event_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="event-type-name" type="string" required=true %}
Name of the event type.
{% endapi-method-parameter %}

{% api-method-parameter name="event-store-name" type="string" required=true %}
Name of the event store.
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
status of :even-type-name \(event-type-1\)
{% endapi-method-response-example-description %}

```
{
    "status": "STOPPED",
    "_links": {
        "self": {
            "href": "https://hostname/event-types/eventtype-1/eventstores/pg/persister/state"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.grnry.io" path="/event-types/:event-type-name/eventstores/:event-store-name/persister/state" %}
{% api-method-summary %}
Update State of a Persister for a Specific Event Type
{% endapi-method-summary %}

{% api-method-description %}
Start or stop the state of a persister for an event type.  
This request requires the role `event_type_edit` and `event_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="event-type-name" type="string" required=true %}
Name of the event type.
{% endapi-method-parameter %}

{% api-method-parameter name="event-store-name" type="string" required=true %}
Name of the event store.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="action" type="string" required=false %}
Updates the status of this persister. Possible values: `START` , `STOP`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "status": "DEPLOYING",
    "_links": {
        "self": {
            "href": "https://hostname/event-types/eventtype-1/eventstores/pg/persister/state"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/event-types/:event-type-name/eventstores/:event-store-name/persister/logs" %}
{% api-method-summary %}
Get Persister Logs for a Specific Event Type
{% endapi-method-summary %}

{% api-method-description %}
Get the logs from a persister of an event type.  
This request requires the role `event_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="event-type-name" type="string" required=true %}
Name of the event-type
{% endapi-method-parameter %}

{% api-method-parameter name="event-store-name" type="string" required=true %}
Name of the event store.
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
Valid value are : 1 .. 5000. Default: 5000.
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

{% api-method method="get" host="https://api.grnry.io" path="/harvester/instances" %}
{% api-method-summary %}
List all Harvester Instances
{% endapi-method-summary %}

{% api-method-description %}
returns a pageable list of all harvesters
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="pagesize" type="integer" required=false %}
Number of harvesters returned, default is 20. Maximum is 250.
{% endapi-method-parameter %}

{% api-method-parameter name="expand" type="string" required=false %}
Show harvester details with expand=harvesters. Add total count of harvesters with expand=totalCount.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="integer" required=false %}
Start offset. Default: 0
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

```
{
   "harvesters":[
      {
         "name":"harvester-1",
         "_links":{
            "self":{
               "href":"https://api.acme.com/harvesters/instances/harvester-1"
            }
         }
      },
      {
         "name":"demo-harvester",
         "_links":{
            "self":{
               "href":"https://api.acme.com/harvesters/instances/demo-harvester"
            }
         }
      }
   ],
   "totalCount":2,
   "_links":{
      "self":{
         "href":"https://api.acme.com/harvesters/instances?search=&offset=0&pagesize=20&expand=totalCount"
      }
   }
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
returns all details of a given harvester
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="harvester-name" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Sample Response
{% endapi-method-response-example-description %}

```
{
    "name": "harvester-demo",
    "displayName": "Harvester Demo",
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
                "href": "https://development.analytics.ventures.syncier.cloud/harvesters/source-types/grnry-jdbc/latest"
            }
        }
    },
    "metadataExtractor": {
        "app": "grnry-data-in-metadata",
        "version": "latest",
        "deploymentConfiguration": {
            "kubernetes": {
                "limits": {
                    "cpu": "500m",
                    "memory": "512Mi"
                },
                "volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
                "requests": {
                    "cpu": "500m",
                    "memory": "512Mi"
                },
                "volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
                "imagepullpolicy": "Always",
                "livenessprobedelay": "120",
                "readinessprobedelay": "120"
            }
        },
        "appConfiguration": {
            "spring": {
                "cloud": {
                    "stream": {
                        "kafka": {
                            "binder": {
                                "autocreatetopics": "true",
                                "autoaddpartitions": "true",
                                "minpartitioncount": "24",
                                "replicationfactor": "3"
                            }
                        },
                        "bindings": {
                            "input": {
                                "consumer": {
                                    "concurrency": "6",
                                    "partitioned": "true"
                                }
                            }
                        }
                    }
                }
            }
        }
    },
    "transform": {
        "app": "grnry-scriptable",
        "version": "latest",
        "deploymentConfiguration": {
            "kubernetes": {
                "limits": {
                    "cpu": "500m",
                    "memory": "512Mi"
                },
                "volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
                "requests": {
                    "cpu": "500m",
                    "memory": "512Mi"
                },
                "volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
                "imagepullpolicy": "Always",
                "livenessprobedelay": "120",
                "readinessprobedelay": "120"
            }
        },
        "appConfiguration": {
            "spring": {
                "cloud": {
                    "stream": {
                        "kafka": {
                            "binder": {
                                "autocreatetopics": "true",
                                "autoaddpartitions": "true",
                                "minpartitioncount": "24",
                                "replicationfactor": "3"
                            }
                        },
                        "bindings": {
                            "input": {
                                "consumer": {
                                    "concurrency": "6",
                                    "partitioned": "true"
                                }
                            }
                        }
                    }
                }
            }
        },
        "language": "groovy",
        "script": "return new String(payload , 'UTF-8');"
    },
    "eventType": {
        "name": "snowplow-a",
        "version": "1"
    },
    "_links": {
        "self": {
            "href": "https://development.analytics.ventures.syncier.cloud/harvesters/instances/harvester-demo"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.grnry.io" path="/harvesters/instances" %}
{% api-method-summary %}
Create Harvester
{% endapi-method-summary %}

{% api-method-description %}
Creates a new harvester instance. Source type and event type are referenced by `name` and `version` in event\_types and source\_types tables. If no configuration properties are set under `sourceType` \(resp. `eventType`\) configuration from these entities will be applied.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="metadataExtractor" type="object" required=false %}
metadata extractor application used by this harvester. Default values for all fields can be specified during harvester api deployment. _Optional_ fields are `app:string` \(registered app in scdf\), `version:string` \(registered app version in scdf\), `deploymentConfiguration:map`, `appConfiguration:map`.
{% endapi-method-parameter %}

{% api-method-parameter name="transform" type="object" required=false %}
scriptable transform application used by this harvester. Default values for all fields can be specified during harvester api deployment. _Optional_ fileds are `app:string` \(registered app in scdf\), `version:string` \(app version registered in scdf\), `deploymentConfiguration:map`, `appConfiguration:map`, `language:string` \(script language\), `script:string` \(script that transforms the data\).
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
technical harvester name. Needs to be unique.
{% endapi-method-parameter %}

{% api-method-parameter name="sourceType" type="object" required=true %}
existing sourceType that this harvester receives data from. _Required_ fields are `name:string` and `version:string`. _Optional_ fields are `configuration:map`, `deployerConfiguration:map`, `appConfiguration:map`
{% endapi-method-parameter %}

{% api-method-parameter name="eventType" type="object" required=true %}
existing eventType that this harvester should process. _Required_ fields are `name:string` and `version:string`.
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
harvester description
{% endapi-method-parameter %}

{% api-method-parameter name="displayName" type="string" required=true %}
human readable name. Needs to be unique
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
            "kubernetes": {
                "requests": {
                    "memory": "512Mi",
                    "cpu": "500m"
                },
                "livenessprobedelay": "120",
                "readinessprobedelay": "120",
                "volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
                "limits": {
                    "cpu": "500m",
                    "memory": "512Mi"
                },
                "volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
                "imagepullpolicy": "Always"
            }
        },
        "appConfiguration": {
            "spring": {
                "cloud": {
                    "stream": {
                        "kafka": {
                            "binder": {
                                "replicationfactor": "3",
                                "autocreatetopics": "true",
                                "minpartitioncount": "24",
                                "autoaddpartitions": "true"
                            }
                        },
                        "bindings": {
                            "input": {
                                "consumer": {
                                    "partitioned": "true",
                                    "concurrency": "6"
                                }
                            }
                        }
                    }
                }
            }
        }
    },
    "transform": {
        "app": "grnry-scriptable",
        "version": "2",
        "deploymentConfiguration": {
            "kubernetes": {
                "readinessprobedelay": "120",
                "volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
                "limits": {
                    "memory": "512Mi",
                    "cpu": "500m"
                },
                "volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
                "requests": {
                    "cpu": "500m",
                    "memory": "512Mi"
                },
                "imagepullpolicy": "Always",
                "livenessprobedelay": "120"
            }
        },
        "appConfiguration": {
            "spring": {
                "cloud": {
                    "stream": {
                        "bindings": {
                            "input": {
                                "consumer": {
                                    "partitioned": "true",
                                    "concurrency": "6"
                                }
                            }
                        },
                        "kafka": {
                            "binder": {
                                "replicationfactor": "3",
                                "autoaddpartitions": "true",
                                "minpartitioncount": "24",
                                "autocreatetopics": "true"
                            }
                        }
                    }
                }
            }
        },
        "language": "groovy",
        "script": "return new String(payload , 'UTF-8');"
    },
    "eventType": {
        "name": "snowplow-a",
        "version": "latest"
    },
    "_links": {
        "self": {
            "href": "https://development.analytics.ventures.syncier.cloud/harvesters/instances/harvester-post"
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
    "timestamp": 1579697549062,
    "message": "Validation failed for argument at index 0 in method: public org.springframework.hateoas.Resource<io.grnry.harvester_api.harvesters.response.HarvesterResponse> io.grnry.harvester_api.harvesters.HarvesterController.addHarvester(io.grnry.harvester_api.harvesters.requests.HarvesterPostRequest), with 1 error(s): [Field error in object 'harvesterPostRequest' on field 'eventType.version': rejected value [null]; codes [NotNull.harvesterPostRequest.eventType.version,NotNull.eventType.version,NotNull.version,NotNull.java.lang.String,NotNull]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [harvesterPostRequest.eventType.version,eventType.version]; arguments []; default message [eventType.version]]; default message [must not be null]] ",
    "details": "uri=/harvesters/instances"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
harvester name already exists
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1579697613983,
    "message": "harvester instance with name harvester-post already exists ",
    "details": "uri=/harvesters/instances"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

