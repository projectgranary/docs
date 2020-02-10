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
* PUT /harvesters/instances/{harvester-name}
* DELETE /harvesters/instances/{harvester-name}
* GET /harvesters/instances/{harvester-name}/state
* POST /harvesters/instances/{harvester-name}/state

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

## API Methods

Consult the [Granary Access Clients Reference](../../operator-reference/identity-and-access-management/granary-access-clients.md#harvester-api) for roles a user needs to interact with Harvester API.

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
{}
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

Possible values for the status attribute in the response body are:

| Status | Description |
| :--- | :--- |
| UNKNOWN | status could not be determined |
| STOPPED | persister is not deployed |
| DEPLOYING | persister is being deployed |
| STOPPING | persister is being stopped |
| RUNNING | persister is running |
| RUNNING\_BUT\_OUTDATED | persister is running but there is a newer version of it in the database |
| FAILED | persister is deployed but not running |

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
{
    "logs": [
        "\tsasl.kerberos.service.name = null",
        "\tsasl.kerberos.ticket.renew.jitter = 0.05",
        "\tsasl.kerberos.ticket.renew.window.factor = 0.8",
        "\tsasl.login.callback.handler.class = null",
        "\tsasl.login.class = null",
        "\tsasl.login.refresh.buffer.seconds = 300",
        "\tsasl.login.refresh.min.period.seconds = 60",
        "\tsasl.login.refresh.window.factor = 0.8",
        "\tsasl.login.refresh.window.jitter = 0.05",
        "\tsasl.mechanism = GSSAPI",
        "\tsecurity.protocol = PLAINTEXT",
        "\tsend.buffer.bytes = 131072",
        "\tsession.timeout.ms = 10000",
        "\tssl.cipher.suites = null",
        "\tssl.enabled.protocols = [TLSv1.2, TLSv1.1, TLSv1]",
        "\tssl.endpoint.identification.algorithm = https",
        "\tssl.key.password = null",
        "\tssl.keymanager.algorithm = SunX509",
        "\tssl.keystore.location = null",
        "\tssl.keystore.password = null",
        "\tssl.keystore.type = JKS",
        "\tssl.protocol = TLS",
        "\tssl.provider = null",
        "\tssl.secure.random.implementation = null",
        "\tssl.trustmanager.algorithm = PKIX",
        "\tssl.truststore.location = null",
        "\tssl.truststore.password = null",
        "\tssl.truststore.type = JKS",
        "\tvalue.deserializer = class de.saly.kafka.crypto.DecryptingDeserializer",
        "",
        "2020-02-10 09:30:02.221  INFO [g-p-pg-snowplow-a-sink,,,] 1 --- [container-3-C-1] org.apache.kafka.clients.Metadata        : Cluster ID: v8AY3bE6Sou7aXjAfipNQQ",
        "2020-02-10 09:30:02.221  INFO [g-p-pg-snowplow-a-sink,,,] 1 --- [container-3-C-1] o.a.k.c.c.internals.AbstractCoordinator  : [Consumer clientId=consumer-6, groupId=g-p-pg-snowplow-a] Discovered group coordinator 10.42.2.218:9092 (id: 2147483647 rack: null)",
        ...
    ]
}
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
technical name of harvester
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
            }
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
Creates a new harvester instance. A technical name for the harvester instance will be derived from given `displayName` by removing all special characters, replacing white spaces with hyphens and limiting the length to 20 characters. Should the created harvester name be already in use the last four characters will be replaced by a suffix of numbers.  
  
Source type and event type are referenced by `name` and `version` in event\_types and source\_types tables. If no configuration properties are set under `sourceType` \(resp. `eventType`\) configuration from these entities will be applied.
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
human readable name. Needs to be unique. A technical name will be derived from it.
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

{% api-method method="put" host="https://api.grnry.io" path="harvesters/instances/:harvester-name" %}
{% api-method-summary %}
Update Harvester Instance
{% endapi-method-summary %}

{% api-method-description %}
Updates a harvester instance. All body parameters are optional. Harvester name filed `name` is not changeable and will be ignored if provided. Empty fields will be set to `""`, missing fields will remain unchanged. It is not possible to replace apps \(`sourceType`, `metadataExtractor`, `transform`\), only their versions and configs are modifiable.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="harvester-name" type="string" required=false %}
name of the harvester that should be updated
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="metadataExtractor" type="object" required=false %}
metadata extractor application used by this harvester. Default values for all fields can be specified during harvester api deployment. _Optional_ fields are `version:string` \(registered app version in scdf\), `deploymentConfiguration:map`, `appConfiguration:map`.
{% endapi-method-parameter %}

{% api-method-parameter name="transform" type="object" required=false %}
scriptable transform application used by this harvester. Default values for all fields can be specified during harvester api deployment. _Optional_ fileds are  `version:string` \(app version registered in scdf\), `deploymentConfiguration:map`, `appConfiguration:map`, `language:string` \(script language\), `script:string` \(script that transforms the data\).
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

{% api-method-response-example httpCode=304 %}
{% api-method-response-example-description %}
Harvester is not modified.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "timestamp": 1580290821033,
    "message": "displayName : string length must be between 3 and 60 but is 101",
    "details": "uri=/harvesters/instances/demo-set-all"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "timestamp": 1580290961327,
    "message": "Access is denied",
    "details": "uri=/harvesters/instances/demo-set-all"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "timestamp": 1580291007569,
    "message": "No harvester found with name 'demo-set-all'.",
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
    "message": "harvester with displayName 'Harvester Post' already exists ",
    "details": "uri=/harvesters/instances/demo-set-all"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host="https://api.grnry.io" path="/harvesters/instances/:harvester-name" %}
{% api-method-summary %}
Delete a Harvester
{% endapi-method-summary %}

{% api-method-description %}
Deletes the given Harvester.  
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
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

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

```
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
Post Harvester Instance State
{% endapi-method-summary %}

{% api-method-description %}
Start or stop the state of the given Harvester.  
This request requires the roles `harvester_read` and `harvester_edit`.
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

```
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

