---
description: Harvester API's event type endpoints
---

# Event Type Endpoints

## Paths

* GET /projects/{project-name}/event-types
* GET /projects/{project-name}/event-types/{event-type-name}
* POST /projects/{project-name}/event-types
* POST /projects/{project-name}/event-types/{event-type-name}
* PUT/projects/{project-name}/event-types/{event-type-name}
* DELETE /projects/{project-name}/event-types/{event-type-name} 
* GET/projects/{project-name}/event-types/{event-type-name}/{version}
* GET /projects/{project-name}/event-types/{event-type-name}/eventstores/{event-store-name}/persister
* PUT /projects/{project-name}/event-types/{event-type-name}/eventstores/{event-store-name}/persister
* GET /projects/{project-name}/event-types/{event-type-name}/eventstores/{event-store-name}/persister/state
* POST /projects/{project-name}/event-types/{event-type-name}/eventstores/{event-store-name}/persister/state
* GET /projects/{project-name}/event-types/{event-type-name}/eventstores/{event-store-name}/persister/logs

## API Methods

Consult the [Granary Access Clients Reference](../../../operator-reference/identity-and-access-management/granary-access-clients.md#harvester-api) for roles a user needs to interact with Harvester API.

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types" method="get" summary="Get all Event Types" %}
{% swagger-description %}
Returns a list of all event types (latest version of each event type) optionally filtered by project name. To get all event types from all projects, leave out 

`projects/{project-name}/`

 in the URL.

\


If 

`projects/{project-name}`

 is present, the project's 

`viewer`

 or 

`editor`

 role is necessary.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project. Filters event types by project. If 

`projects/{projectName}/`

 is missing, all event types from all projects will be retrieved but with limited response bodies.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="expand" type="string" %}
Expand the response with 

`totalCount`

, or 

`persisters`

 to show either the count or the peristers of different event-type-names. Default is 

`""`

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="search" type="string" %}
Filter event types by displaynames containing this search term Default is 

`""`

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="pagesize" type="number" %}
Number of event types returned per page. Default is 

`20`

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="number" %}
Offset of the requested page. Default is 

`0`

. Must be a whole multiple of 

`pagesize`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/event-types?search=&offset=0&pagesize=20&expand="
        }
    },
    "eventTypes": [
       {
            "name": "snowplow-tracking",
            "displayName": "snowplow-tracking",
            "createdAt": "2021-09-24T08:38:50.848Z",
            "createdBy": {
                "name": "user",
                "email": "user@grnry.io"
            },
            "modifiedAt": "2021-09-24T08:40:48.922Z",
            "modifiedBy": {
                "name": "other user",
                "email": "other-user@grnry.io"
            },
            "projectName": "global",
            "version": 1,
            "projectName": "tracking",
            "editor": "event_type_data_in_edit",
            "consumer": null,
            "type": "data_in",
            "correlationIdExpression": "#safeJsonPath(#safeJsonPath(payload,'body.data.ue_pr'),'data.data.environment.cid')?:'NO_CORRELATION_ID'",
            "eventIdExpression": "#randomUUID()",
            "timestampExpression": "#nowMillis()",
            "topicName" : "grnry_data_in_snowplow-tracking",
            "retentionMs": 3456000000,
            "partitionCount": 32,
            "replication": 2,
            "eventstoreTTL": "P100Y",
            "description": "",
            "physicalLocations": [...],
            "source": [...],
            "schema": null,
            "inputs": {
                "harvesters": [...],
                "belts": [...]
            },
            "outputs": {
                "persisters": [...],
                "belts": [...]
            },
            "_links": {
                "self": {
                    "href": "https://hostname/projects/global/event-types/snowplow-tracking/1"
                }
            }
        },
        {
            "name": "call-records",
            "displayName": "call-records",
            "projectName": "global",
            "version" : 10,
            ....
            "_links": {
                "self": {
                    "href": "https://hostname/projects/global/event-types/call-records/10"
                }
            }
        },
        {
            "name": "invoices",
            "displayName": "invoices",
            "projectName": "global",
            "version" : 5,
            ....
            "_links": {
                "self": {
                    "href": "https://hostname/projects/global/event-types/invoices/5"
                }
            }
        }
    ]
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Invalid query parameter value." %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'pagesize' must be a natural number but is '-1'.",
    "details": "uri=/projects/global/event-types"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/event-types"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name" method="get" summary="Get all versions of an Event Type" %}
{% swagger-description %}
Get all versions of a given event type. For a full response model 

`/projects/{project-name}`

 is required and the 

`editor`

 or 

`viewer`

 role for that project is needed.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project. Filters event types by project. If 

`projects/{projectName}/`

 is missing, event type is returned with limited response body.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" %}
Name of the event type.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token
{% endswagger-parameter %}

{% swagger-response status="200" description="All versions of the requested :event-type-name (postman-event-type)." %}
```
{
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/event-types/postman-event-type"
        }
    },
    "eventTypes": [
        {
            "name": "postman-event-type",
            "displayName": "postman-event-type",
            "createdAt": "2021-09-24T08:38:50.848Z",
            "createdBy": {
                "name": "user",
                "email": "user@grnry.io"
            },
            "modifiedAt": "2021-09-24T08:40:48.922Z",
            "modifiedBy": {
                "name": "other user",
                "email": "other-user@grnry.io"
            },
            "projectName": "global",
            "version": "1",
            ....
            "_links": {
                "self": {
                    "href": "https://hostname/projects/global/event-types/postman-event-type/1"
                }
            }
        },
        {
            "name": "postman-event-type",
            "displayName": "postman-event-type",
            "projectName": "global",
            "version": "2",
            ...
            "_links": {
                "self": {
                    "href": "https://hostname/projects/global/event-types/postman-event-type/2"
                }
            }
        },
        ....
        {
            "name": "postman-event-type",
            "displayName": "postman-event-type",
            "projectName": "global",
            "version": "12",
            ....
            "_links": {
                "self": {
                    "href": "https://hostname/projects/global/event-types/postman-event-type/4"
                }
            }
        }
    ]
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/event-types/adobe-s3"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/adobe-s3"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="No event-type found with that name (case sensitive)." %}
```
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Event Type 'new-entity' not found.",
    "details": "uri=/projects/global/event-types/new-entity"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name" method="post" summary="Create an Event Type" %}
{% swagger-description %}
To create an event type your user needs to have the project's 

`editor`

 role.

\




\


When you create a 

`data_in`

 event type (default), the system will automatically create a persister using the persister default configuration. On other types besides 

`data_in`

 (like 

`ttl`

/

`ttn`

) it will just create the event type and all persister endpoints will return 

`404 - not found`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the event-type belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" %}
unique event type name
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="source" type="array" %}
Source as in the server host of physical location references. Only for requests of type 

`custom`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="schema" type="object" %}
JSON schema for event type data. kafka schema id or escaped string of schema needed. Only for requests of type 

`live_segment`

 and 

`custom`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="physicalLocations" type="array" %}
Reference to data's physical location. Allowed location types are: 

`database`

, 

`stream`

. Only for requests of type 

`profile`

 and 

`custom`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="deletionExpression" type="string" %}
Optional (boolean) SpringEL to determine if a deletion should occur. Default: 

`''`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="type" type="string" %}
The type of this event type. Allowed values are: 

`data_in`

 , 

`ttl`

, 

`ttn`

 , 

`profile`

, 

`segment`

,

`live_segment`

, 

`custom`

. Defaults to 

`data_in`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="eventstoreTTL" type="string" %}
time-to-live configuration for all eventstores in ISO 8601 Duration format.

\


Default P100Y (100 years)
{% endswagger-parameter %}

{% swagger-parameter in="body" name="replication" type="integer" %}
Kafka topic replication factor. 

\


Default: 3

\


Note: this setting is only available on event type creation and can not be updated afterwards.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="partitionCount" type="integer" %}
Kafka topic partition count. 

\


Default: 32

\


Note: This settting is only available on event type creation and can not be updated afterwards.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="retentionMs" type="number" %}
kafka topic retention in milliseconds. 

\


Used for kafka topic config of segment.ms and retention.ms.

\


Default: 345600000 ( 4 days)

\


Note: This setting is only available on event type creation and can not be updated afterwards.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="string" %}
description of the eventType.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="eventIdExpression" type="string" %}
The SpringEL expression to create the grnry-event-id.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="correlationIdExpression" type="string" %}
The SpringEL expression to create the grnry-correlation-id.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="timestampExpression" type="string" %}
The SpringEL Expression to create the grnry-event-timestamp. Expression has to return milliseconds since 1.1.1970 UTC.

\


Default: #nowMillis()
{% endswagger-parameter %}

{% swagger-parameter in="body" name="displayName" type="string" %}
Displayname of the new event type
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "name": "event-type-4",
    "version": "1",
    
    ...
    
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/event-types/event-type-4/1"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="If required parameters are missing or parameters are invalid." %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'displayName' must not be empty.",
    "details":"uri=/projects/global/event-types"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/event-types"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types"
}
```
{% endswagger-response %}

{% swagger-response status="409" description="If displayName not unique (case insensitive)." %}
```
{
    "timestamp": 1587303130850,
    "type": "entity_already_exists"
    "message": "Event Type with displayName 'adobe-s3' already exists.",
    "details": "uri=/projects/global/event-types"
}
```
{% endswagger-response %}

{% swagger-response status="500" description="Persister already exists for the eventy type." %}
```
{
    "timestamp":1586949269381,
    "type": "unexpected_error",
    "message":"An unexpected error occurred.",
    "details":"uri=/projects/global/event-types"
}
```
{% endswagger-response %}
{% endswagger %}



#### Allowed Parameters in "Create an Event Type" request by Event Type

| Event Type   | Physical Location | Source | Schema |
| ------------ | :---------------: | :----: | :----: |
| Data In      |         -         |    -   |    -   |
| Profile      |         +         |    -   |    -   |
| Live Segment |         -         |    -   |    +   |
| Segment      |         -         |    -   |    -   |
| TTL/TTN      |         -         |    -   |    -   |

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name" method="put" summary="Update an Event Type" %}
{% swagger-description %}
Fully or partially updates an event type. If no delta was recognized, no update will be made and HTTP 304 will be returned. 

\




\


To create an event type your user needs to have the project's 

`editor`

 role.

\




\


Immutable fields (like type, partitionCount,.. ) can be part of the request body, but their values need to match the current values, otherwise an error is raised.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the event-type belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" %}
Name of the event type
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="schema" type="object" %}
JSON schema for event type. Kafka schema id or escaped string of schema needed.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="deletionExpression" type="string" %}
(Optional) SpringEL to determine if a deletion should occur.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="displayName" type="string" %}
The displayname used in the UI
{% endswagger-parameter %}

{% swagger-parameter in="body" name="timestampExpression" type="string" %}
The SpringEL expression to create the grnry-event-timestamp. It has to return milliseconds since 1.1.1970 UTC.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="string" %}
The description of the event type
{% endswagger-parameter %}

{% swagger-parameter in="body" name="eventstoreTTL" type="string" %}
time-to-live configuration for all eventstores in ISO 8601 duration format.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="eventIdExpression" type="string" %}
The SpringEL expression to create the grnry-event-id
{% endswagger-parameter %}

{% swagger-parameter in="body" name="correlationIdExpression" type="string" %}
The SpringEL expression to create the grnry-correlation-id.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
        "name": "foo-test",
        "displayName": "my-test-type",
        "createdAt": "2021-09-24T08:38:50.848Z",
        "createdBy": {
            "name": "user",
            "email": "user@grnry.io"
        },
        "modifiedAt": "2021-09-24T08:40:48.922Z",
        "modifiedBy": {
            "name": "other user",
            "email": "other-user@grnry.io"
        },
        "projectName": "global",        
        "version": 2,
        "topicName" : "grnry_data_in_foo-test",
        "correlationIdExpression": "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].correlationId')?:'NO_CORRELATION_ID'",
        "eventIdExpression": "#randomUUID()",
        "timestampExpression": "#nowMillis()",
        "retentionMs": 3456000000,
        "partitionCount": 32,
        "replication": 2,
        "eventstoreTTL": "P100Y",
        "description": "",
        "_links": {
            "self": {
                "href": "https://hostname/projects/global/event-types/foo-test/2"
            }
        }
    },
```
{% endswagger-response %}

{% swagger-response status="400" description="Invalid parameter." %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'type' must be empty or 'ttl' but is 'data_in'.",
    "details":"uri=/projects/global/event-types/test-event-type-2"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/event-types/test-event-type"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name" method="delete" summary="Delete an Event Type" %}
{% swagger-description %}
Deletes all versions of the given event type, if it is not used by registered belts. To delete an event type, a user must have the project's 

`editor`

 role.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the event-type belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" %}
name of the event type to be deleted
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token
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
    "details": "uri=/projects/global/event-types/13213"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type-tp"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="No event type with given name (case sensitive) found." %}
```
{
    "timestamp":1586949280969,
    "type": "entity_not_found",
    "message": "Event Type 'test-delete' not found.",
    "details":"uri=/projects/global/event-types/test-delete"
}
```
{% endswagger-response %}

{% swagger-response status="412" description="If there are still belts and/or harvesters referencing the event-type, the deletion will fail." %}
```
{
    "timestamp": 1574854606738,
    "type": "entity_in_use"
    "message": "Cannot delete Event Type 'event-type-delete', it is still being referenced by belts: 
    [https://hostname/belts/11,
     https://hostname/belts/12,
     https://hostname/belts/13,
     https://hostname/belts/14,
     https://hostname/belts/15,
     https://hostname/belts/16,
     https://hostname/belts/17,
     https://hostname/belts/18,
     https://hostname/belts/19].",
    "details": "uri=/projects/global/event-types/test-event-type-delete"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name/:version" method="get" summary="Get a Specific Version of an Event Type" %}
{% swagger-description %}
Get one version of an event type.

\


Version should be "latest" or a valid number greater than or equals to 1.

\


To find this event-type in all projects leave out 

`projects/{project-name}/`

 in the URL. If 

`projects/{project-name}`

 is present, the respective project's 

`viewer`

 or 

`editor`

 role is necessary.
{% endswagger-description %}

{% swagger-parameter in="path" name="projectName" type="string" %}
Name of project the event-type belongs to. If 

`projects/{projectName}/`

 is missing, the event type is returned with limited response body.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="version" type="string" %}
The version of the event type, or "latest" to get the latest version
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" %}
Name of the event type
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="export" type="string" %}
If "true" or "True", the event type response will only contain properties a POST body must contain to create this exact event type. All properties set to default values and all api-generated properties will be omitted. Default is 

`""`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "name": "snowplow-webtracking",
    "displayName": "snowplow-webtracking",
    "createdAt": "2021-09-24T08:38:50.848Z",
    "createdBy": {
        "name": "user",
        "email": "user@grnry.io"
    },
    "modifiedAt": "2021-09-24T08:40:48.922Z",
    "modifiedBy": {
        "name": "other user",
        "email": "other-user@grnry.io"
    },
    "projectName": "global",
    "version": 1,
    "projectName": "webtracking",
    "type": "data_in",
    "correlationIdExpression": "#safeJsonPath(#safeJsonPath(payload,'body.data.ue_pr'),'data.data.environment.cid')?:'NO_CORRELATION_ID'",
    "eventIdExpression": "#randomUUID()",
    "timestampExpression": "#nowMillis()",
    "deletionExpression": "",
    "topicName": "grnry_data_in_snowplow-webtracking",
    "retentionMs": 3456000000,
    "partitionCount": 32,
    "replication": 2,
    "eventstoreTTL": "P100Y",
    "description": "",
    "schema": null,
    "source": [
         {
             "postgres": {
                 "type": "database",
                 "connectionUrl": "jdbc:postgresql://hostname:5432/database",
                 "connectionUser": "user",
                 "connectionSecretRef":"webtracking-jdbc"
             }
         },
         {
             "kafka": {
                 "type": "stream",
                 "connectionUrl": "http://hostname:port",
                 "connectionUser": "",
                 "connectionSecretRef": "grnry-kafka"                 
             }
         }
    ],
    "physicalLocations": [
        {
            "locationName": "webtracking.eventstore_snowplow-webtracking",
            "source": "postgresql",
            "type": "database"
        },
        {
            "locationName": "grnry_data_in_snowplow-webtracking",
            "source": "kafka",
            "type": "stream"
        }
    ],
    "inputs": {
        "harvesters": [
            {
                "projectName": "global",
                "_links": {
                    "self": {
                        "href": "https://grnry.io/projects/global/harvesters/instances/Harvester-1?expand=&export="
                    }
                },
                "id": "Harvester-1"
            }
        ],
        "belts": [
            {
                "projectName": "global",
                "_links": {
                    "self": {
                        "href": "https://grnry.io/projects/global/belts/1?export="
                    }
                },
                "id": "1"
            }
        ]
    },
    "outputs": {
        "persisters": [
            {
                "eventTypeName": "snowplow-webtracking",
                "eventStoreType": "pg",
                "projectName": "global",
                "_links": {
                    "self": {
                        "href": "https://grnry.io/projects/global/event-types/snowplow-webtracking/eventstores/pg/persister?expand=&export="
                    }
                }
            }
        ],
        "belts": [
            {
                "projectName": "global",
                "_links": {
                    "self": {
                        "href": "https://grnry.io/projects/global/belts/1?export="
                    }
                },
                "id": "2"
            }
        ]
    },
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/event-types/snowplow-webtracking/1"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="If the version is not latest or a valid number greater or equals to 1." %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'version' must be a natural number or 'latest' but is '-1'.",
    "details":"uri=/projects/global/event-types/test-event-type/invalidVersion"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/event-types/adobe-s3/latest"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type-partial/latest"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="No event type with given name (case sensitive) found for given version." %}
```
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Event Type 'test-event-type' with version '4' not found.",
    "details":"uri=/projects/global/event-types/test-event-type/4"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name/eventstores/:event-store-name/persister" method="get" summary="Get Persister for a Specific Event Type" %}
{% swagger-description %}
Get the persister of an event type (only available on "data_in" event types).

\


This request requires the role either 

`viewer`

or 

`editor`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="projectName" type="string" %}
Name of project the event-type belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-store-name" type="string" %}
Name of the event store.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" %}
Name of the event type.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="export" type="string" %}
if set to 

`"true"`

 (not case sensitive), the persister response will only contain properties that are not set to default values or generated by the api. Default is 

`""`

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="expand" type="string" %}
Expand the response with 

`state`

 to show the persister state. Default is 

`""`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="requested :event-type-name (eventtype-1) , :event-store-name (pg)" %}
```
{
    "streamName": "grnry-p-pg-postman-eventtype-1",
    "projectName": "global",
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
            "href": "https://hostname/projects/global/event-types/eventtype-1/eventstores/pg/persister"
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
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/pg/persister"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type-partial/eventstores/pg/persister"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Event store, event type not found (expected behaviour on event types other than of type "data_in")" %}
```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Persister 'adobe-s3' not found.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/pgd/persister"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name/eventstores/:event-store-name/persister" method="put" summary="Update Persister Config for a Specific Event Type" %}
{% swagger-description %}
Updates the persister configuration of a specific event type.

\


This request requires the

`editor`

 role of the project.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the event-type belongs to.

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" %}
Name of the event type.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-store-name" type="string" %}
Name of the event store.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="imported" type="string" %}
If set to 

`"true"`

 (not case sensitive), the persister default values will be merged into the provided PUT body while keeping the custom values if provided.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="deployerConfig" type="object" %}
A map containing all deployer configuration options.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="appConfig" type="object" %}
A Map containing all application configuration parameters
{% endswagger-parameter %}

{% swagger-parameter in="body" name="appVersion" type="string" %}
The updated stream app version. This version has to be present in the backend spring cloud data flow server, otherwise the update will fail.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "streamName": "grnry-p-pg-postman-eventtype-1",
    "projectName": "global",
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
            "href": "https://hostname/projects/global/event-types/eventtype-1/eventstores/pg/persister?expand="
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="302" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="304" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="400" description="Invalid parameters" %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'event_type_name' cannot be null",
    "details":"uri=/projects/global/event-types/event-type-empty-update-persister-config/eventstores/pg/persister"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/pg/persister"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type-partial/eventstores/pg/persister"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="500" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="503" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name/eventstores/:event-store-name/persister/state" method="get" summary="Get State of a Persister for a Specific Event Type" %}
{% swagger-description %}
Get the current state of a persister for an event type.

\


This request requires an 

`editor`

 or 

`viewer`

 role of the project.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the event-type belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" %}
Name of the event type.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-store-name" type="string" %}
Name of the event store.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "status": "STOPPED",
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/event-types/eventtype-1/eventstores/pg/persister/state"
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
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/pg/persister/state"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type-partial/eventstores/pg/persister/state"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Event store, event type not found." %}
```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Persister 'adobe-s3' not found.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/pdg/persister/state"
}
```
{% endswagger-response %}
{% endswagger %}

Possible values for the status attribute in the response body are:

| Status               | Description                                                             |
| -------------------- | ----------------------------------------------------------------------- |
| UNKNOWN              | status could not be determined                                          |
| STOPPED              | persister is not deployed                                               |
| DEPLOYING            | persister is being deployed                                             |
| STOPPING             | persister is being stopped                                              |
| RUNNING              | persister is running                                                    |
| RUNNING_BUT_OUTDATED | persister is running but there is a newer version of it in the database |
| FAILED               | persister is deployed but not running                                   |

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name/eventstores/:event-store-name/persister/state" method="post" summary="Start/Stop Persister for a Specific Event Type" %}
{% swagger-description %}
Start or stop the state of a persister for an event type.

\


This request requires the  

`editor`

 role of the project.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the event-type belongs to.

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" %}
Name of the event type.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-store-name" type="string" %}
Name of the event store.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="action" type="string" %}
Updates the status of this persister. Possible values: 

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
            "href": "https://hostname/projects/global/event-types/eventtype-1/eventstores/pg/persister/state"
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
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/pg/persister/state"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type-partial/eventstores/pg/persister/state"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Event store, event type not found." %}
```
{
     "timestamp": 1587303625038,
     "type": "entity_not_found",
     "message": "Persister 'adobe-s3' not found.",
     "details": "uri=/projects/global/event-types/adobe-s3/eventstores/pdg/persister/logs"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name/eventstores/:event-store-name/persister/logs" method="get" summary="Get Persister Logs for a Specific Event Type" %}
{% swagger-description %}
Get the logs from a persister of an event type.

\


This request requires the 

`editor`

 or 

`viewer`

 role of the project.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Name of project the event-type belongs to. 

\




_Backwards compatibility:_

 

\


If 

`projects/{project-name}/`

 is missing, URL will be treated like 

`projects/global/...`

 scoping the request to the 'global' project.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" %}
Name of the event-type
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-store-name" type="string" %}
Name of the event store.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="lines" type="integer" %}
The last x lines of the log (if available).

\


Valid value are : 

`1 .. 500`

. Default: 

`500`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "logs": [
        "2021-08-16 12:30:22.156  INFO [g-p-pg-event-type-sink,,,] 1 --- [-write-health-1] o.a.k.c.c.internals.AbstractCoordinator  : [Consumer clientId=healthindicator-kafka2d1310a3-f25f-420f-8e81-e2cb71ec8218, groupId=healthindicator-kafka-36959226-28ea-4571-b7f1-386d05e2a21d] (Re-)joining group",
        "2021-08-16 12:31:22.156  INFO [g-p-pg-event-type-sink,,,] 1 --- [-write-health-1] o.a.k.c.c.internals.AbstractCoordinator  : [Consumer clientId=healthindicator-kafka2d1310a3-f25f-420f-8e81-e2cb71ec8218, groupId=healthindicator-kafka-36959226-28ea-4571-b7f1-386d05e2a21d] (Re-)joining group"
    ]
}
```
{% endswagger-response %}
{% endswagger %}
