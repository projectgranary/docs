---
description: Harvester API's event type endpoints
---

# Event Type Endpoints

## Paths

* GET /projects/{project-name}/event-types
* GET /projects/{project-name}/event-types/{event-type-name}
* GET/projects/{project-name}/event-types/{event-type-name}/{version}
* POST /projects/{project-name}/event-types
* POST /projects/{project-name}/event-types/{event-type-name}
* PUT/projects/{project-name}/event-types/{event-type-name}
* DELETE /projects/{project-name}/event-types/{event-type-name}&#x20;

## API Methods

Consult the [Granary Access Clients Reference](../../../../operator-reference/identity-and-access-management/granary-access-clients.md#harvester-api) for roles a user needs to interact with Harvester API.

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

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="expand" type="string" %}
Expand the response with 

`inputs`

, 

`outputs`

, or 

`persisters.`

  The

`inputs`

 value will add a list of Granary components producing this event type while 

`outputs`

 will add a list of Granary components consuming this event type. The 

`persisters`

 parameter will give more information about the corresponding persisters including their state. Default is 

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
            "href": "https://hostname/projects/global/event-types?search=&offset=0&pagesize=20&expand=inputs,outputs"
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

{% swagger-parameter in="path" name="event-type-name" type="string" required="true" %}
Name of the event type.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
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

{% swagger-parameter in="path" name="version" type="string" required="true" %}
The version of the event type, or "latest" to get the latest version
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" required="true" %}
Name of the event type
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
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

{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name" method="post" summary="Create an Event Type" %}
{% swagger-description %}
To create an event type your user needs to have the project's 

`editor`

 role.

\




\


When you create a 

`data_in`

 & 

`live_segment`

 event type, the system will automatically create a persister using the persister default configuration. On other types besides 

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

{% swagger-parameter in="path" name="event-type-name" type="string" required="true" %}
Unique event type name.  If not provided, it is derived from body parameter 

`displayName`

.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="displayName" type="string" required="true" %}
Display name of the new event type
{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="string" %}
description of the eventType.
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

{% swagger-parameter in="body" name="source" type="array" %}
Source as in the server host of physical location references. Only for requests of type 

`custom`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="schema" type="object" required="true" %}
JSON schema for event type data. Escaped string of schema needed. Only for requests of type 

`live_segment`

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


Default: 

`345600000`

 (4 days)

\


Note: This setting is only available on event type creation and can not be updated afterwards.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="correlationIdExpression" type="string" required="true" %}
The SpringEL expression to create the grnry-correlation-id. Only valid for type 

`data_in`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="eventIdExpression" type="string" required="true" %}
The SpringEL expression to create the grnry-event-id. Only valid for type 

`data_in`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="timestampExpression" type="string" %}
The SpringEL Expression to create the grnry-event-timestamp. Expression has to return milliseconds since 1.1.1970 UTC.

\


Default: 

`#nowMillis()`

. Only valid for type 

`data_in`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="deletionExpression" type="string" %}
Optional (boolean) SpringEL to determine if a deletion should occur. Default: 

`''`

. Only valid for type 

`data_in`

.
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



The following table lists parameters in "Create an Event Type" request which mutability depends on the Event Type chosen:

| Event Type   | Physical Location | Source | Schema | <ul><li>Expressions</li></ul> |
| ------------ | :---------------: | :----: | :----: | ----------------------------- |
| Data In      |         -         |    -   |    -   | +                             |
| Profile      |         +         |    -   |    -   | -                             |
| Live Segment |         -         |    -   |    +   | -                             |
| Segment      |         -         |    -   |    -   | -                             |
| TTL/TTN      |         -         |    -   |    -   | -                             |



{% swagger baseUrl="https://api.grnry.io" path="/projects/{project-name}/event-types/:event-type-name" method="put" summary="Update an Event Type" %}
{% swagger-description %}
Fully or partially updates an event type. If no delta was recognized, no update will be made and HTTP 304 will be returned. \
\
To create an event type your user needs to have the project's `editor` role.\
\
Immutable fields (like type, partitionCount,.. ) can be part of the request body, but their values need to match the current values, otherwise an error is raised.

If field other than `displayName` or `description` is changed, a new version of the Event Type is created.
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

{% swagger-parameter in="path" name="event-type-name" type="string" required="true" %}
Name of the event type
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="displayName" type="string" %}
The displayname used in the UI
{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="string" %}
The description of the event type
{% endswagger-parameter %}

{% swagger-parameter in="body" name="schema" type="object" %}
JSON schema for event type. Escaped string of schema needed. Only valid for type 

`live_segment`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="correlationIdExpression" type="string" %}
The SpringEL expression to create the 

`grnry-correlation-id`

. Only valid for type 

`data_in`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="eventIdExpression" type="string" %}
The SpringEL expression to create the grnry-event-id. Only valid for type 

`data_in`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="deletionExpression" type="string" %}
(Optional) SpringEL to determine if a deletion should occur. Only valid for type 

`data_in`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="timestampExpression" type="string" %}
The SpringEL expression to create the grnry-event-timestamp. It has to return milliseconds since 1.1.1970 UTC. Only valid for type 

`data_in`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="eventstoreTTL" type="string" %}
time-to-live configuration for all eventstores in ISO 8601 duration format.
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

{% swagger-parameter in="path" name="event-type-name" type="string" required="true" %}
name of the event type to be deleted
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
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
