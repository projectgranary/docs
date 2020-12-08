---
description: Harvester API's event type endpoints
---

# Event Type Endpoints

## Paths

* GET /event-types
* GET /event-types/{event-type-name}
* POST /event-types
* POST /event-types/{event-type-name}
* PUT/event-types/{event-type-name}
* DELETE /event-types/{event-type-name} 
* GET/event-types/{event-type-name}/{version}
* GET /event-types/{event-type-name}/eventstores/{event-store-name}/persister
* PUT /event-types/{event-type-name}/eventstores/{event-store-name}/persister
* GET /event-types/{event-type-name}/eventstores/{event-store-name}/persister/state
* POST /event-types/{event-type-name}/eventstores/{event-store-name}/persister/state
* GET /event-types/{event-type-name}/eventstores/{event-store-name}/persister/logs

## API Methods

Consult the [Granary Access Clients Reference](../../../operator-reference/identity-and-access-management/granary-access-clients.md#harvester-api) for roles a user needs to interact with Harvester API.

{% api-method method="get" host="https://api.grnry.io" path="/event-types" %}
{% api-method-summary %}
Get all Event Types
{% endapi-method-summary %}

{% api-method-description %}
Returns a list of all event types \(latest version of each event type\)  
This request will return all event types, which `consumer` or `editor` matches the or one of the requester's role\(s\). if `consumer` is null, every authenticated user is authorized to see the entity.
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
Filter event types by displaynames containing this search term Default is `""`.
{% endapi-method-parameter %}

{% api-method-parameter name="pagesize" type="number" required=false %}
Number of event types returned per page. Default is `20`.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="number" required=false %}
Offset of the requested page. Default is `0`. Must be a whole multiple of `pagesize`.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "_links": {
        "self": {
            "href": "https://hostname/event-types?search=&offset=0&pagesize=20&expand="
        }
    },
    "eventTypes": [
       {
            "name": "snowplow-tracking",
            "displayName": "snowplow-tracking",
            "version": 1,
            "editor": "event_type_data_in_edit",
            "consumer": null,
            "type": "data_in",
            "correlationIdExpression": "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].correlationId')?:'NO_CORRELATION_ID'",
            "eventIdExpression": "#randomUUID()",
            "timestampExpression": "#nowMillis()",
            "topicName" : "grnry_data_in_snowplow-tracking",
            "retentionMs": 3456000000,
            "partitionCount": 32,
            "replication": 2,
            "eventstoreTTL": "P100Y",
            "description": "",
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/snowplow-tracking/1"
                }
            }
        },
        {
            "name": "call-records",
            "displayName": "call-records",
            "version" : 10,
            ....
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/call-records/10"
                }
            }
        },
        {
            "name": "invoices",
            "displayName": "invoices",
            "version" : 5,
            ....
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/invoices/5"
                }
            }
        }
    ]
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
    "details": "uri=/event-types"
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
    "details": "uri=/event-types"
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
    "details":"uri=/event-types"
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
This request will return all versions, if  `consumer` or `editor` matches the requester's role. If `consumer` is null, every authenticated user is authorized.
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

```text
{
    "_links": {
        "self": {
            "href": "https://hostname/event-types/postman-event-type"
        }
    },
    "eventTypes": [
        {
            "name": "postman-event-type",
            "displayName:" "postman-event-type",
            "version": "1",
            ....
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/postman-event-type/1"
                }
            }
        },
        {
            "name": "postman-event-type",
            "displayName:" "postman-event-type",
            "version": "2",
            ...
            "_links": {
                "self": {
                    "href": "https://hostname/event-types/postman-event-type/2"
                }
            }
        },
        ....
        {
            "name": "postman-event-type",
            "displayName:" "postman-event-type"
            "version": "12",
            ....
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

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/event-types/adobe-s3"
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
    "details":"uri=/event-types/adobe-s3"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
No event-type found with that name \(case sensitive\).
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Event Type 'new-entity' not found.",
    "details": "uri=/event-types/new-entity"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.grnry.io" path="/event-types/:event-type-name" %}
{% api-method-summary %}
Create an Event Type
{% endapi-method-summary %}

{% api-method-description %}
Any authorized user is authorized to create event types. Be aware that you will not be able to update the entity afterwards if your account does not have the role set in `editor`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="event-type-name" type="string" required=false %}
unique event type name
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="consumer" type="string" required=false %}
The name of user group allowed to consume this event type. If not set, there's no restriction applied and any authenticated user can view event-type details and also consume the data with belts. Defaults to `null`
{% endapi-method-parameter %}

{% api-method-parameter name="type" type="string" required=false %}
The type of this event type. Allowed values are: `data_in` , `ttl`. Defaults to `data_in`
{% endapi-method-parameter %}

{% api-method-parameter name="editor" type="string" required=false %}
The name of user group allowed to edit this event type. Defaults to `event_type_<type>_edit`.
{% endapi-method-parameter %}

{% api-method-parameter name="eventstoreTTL" type="string" required=false %}
time-to-live configuration for all eventstores in ISO 8601 Duration format.  
Default P100Y \(100 years\)
{% endapi-method-parameter %}

{% api-method-parameter name="replication" type="integer" required=false %}
Kafka topic replication factor.   
Default: 3  
Note: this setting is only available on event type creation and can not be updated afterwards.
{% endapi-method-parameter %}

{% api-method-parameter name="partitionCount" type="integer" required=false %}
Kafka topic partition count.   
Default: 32  
Note: This settting is only available on event type creation and can not be updated afterwards.
{% endapi-method-parameter %}

{% api-method-parameter name="retentionMs" type="number" required=false %}
kafka topic retention in milliseconds.   
Used for kafka topic config of segment.ms and retention.ms.  
Default: 345600000 \( 4 days\)  
Note: This setting is only available on event type creation and can not be updated afterwards.
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
description of the eventType.
{% endapi-method-parameter %}

{% api-method-parameter name="eventIdExpression" type="string" required=true %}
The SpringEL expression to create the grnry-event-id.
{% endapi-method-parameter %}

{% api-method-parameter name="correlationIdExpression" type="string" required=true %}
The SpringEL expression to create the grnry-correlation-id.
{% endapi-method-parameter %}

{% api-method-parameter name="timestampExpression" type="string" required=false %}
The SpringEL Expression to create the grnry-event-timestamp. Expression has to return milliseconds since 1.1.1970 UTC.  
Default: \#nowMillis\(\)
{% endapi-method-parameter %}

{% api-method-parameter name="displayName" type="string" required=true %}
Displayname of the new event type
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
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

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If required parameters are missing or parameters are invalid.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'displayName' must not be empty.",
    "details":"uri=/event-types"
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
    "details": "uri=/event-types"
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
    "details":"uri=/event-types"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
If displayName not unique \(case insensitive\).
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587303130850,
    "type": "entity_already_exists"
    "message": "Event Type with displayName 'adobe-s3' already exists.",
    "details": "uri=/event-types"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Persister already exists for the eventy type.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949269381,
    "type": "unexpected_error",
    "message":"An unexpected error occurred.",
    "details":"uri=/event-types"
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
This requires the requester to assume roles that match the `editor` field. If no delta was recognized, no update will be made and HTTP 304 will be returned.  
  
Immutable fields \(like type, partitionCount,.. \) can be part of the request body, but their values need to match the current values, otherwise an error is raised.
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

{% api-method-body-parameters %}
{% api-method-parameter name="displayName" type="string" required=false %}
The displayname used in the UI
{% endapi-method-parameter %}

{% api-method-parameter name="timestampExpression" type="string" required=false %}
The SpringEL expression to create the grnry-event-timestamp. It has to return milliseconds since 1.1.1970 UTC.
{% endapi-method-parameter %}

{% api-method-parameter name="description" type="string" required=false %}
The description of the event type
{% endapi-method-parameter %}

{% api-method-parameter name="eventstoreTTL" type="string" required=false %}
time-to-live configuration for all eventstores in ISO 8601 duration format.
{% endapi-method-parameter %}

{% api-method-parameter name="eventIdExpression" type="string" required=false %}
The SpringEL expression to create the grnry-event-id
{% endapi-method-parameter %}

{% api-method-parameter name="correlationIdExpression" type="string" required=false %}
The SpringEL expression to create the grnry-correlation-id.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
        "name": "foo-test",
        "displayName": "my-test-type"
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
                "href": "https://hostname/event-types/foo-test/2"
            }
        }
    },
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid parameter.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'type' must be empty or 'ttl' but is 'data_in'.",
    "details":"uri=/event-types/test-event-type-2"
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
    "details": "uri=/event-types/test-event-type"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Missing roles to access this resource.
{% endapi-method-response-example-description %}

```text
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/event-types/test-event-type"
}
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
Deletes all versions of the given event type, if it is not used by registered belts. This request requires the role matching `editor` field.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="event-type-name" type="string" required=true %}
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
    "details": "uri=/event-types/13213"
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
    "details":"uri=/event-types/test-event-type-tp"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
No event type with given name \(case sensitive\) found.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949280969,
    "type": "entity_not_found",
    "message": "Event Type 'test-delete' not found.",
    "details":"uri=/event-types/test-delete"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=412 %}
{% api-method-response-example-description %}
If there are still belts and/or harvesters referencing the event-type, the deletion will fail.
{% endapi-method-response-example-description %}

```text
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
This request requires the role matching `consumer` or `editor` field of the event type. If `consumer` is not set, any authenticated user is authorized.
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

```text
{
    "name": "snowplow-webtracking",
    "displayName: "snowplow-webtracking",
    "version": 1,
    "correlationIdExpression": "#safeJsonPath(#safeJsonPath(payload, 'body'), 'data[0].correlationId')?:'NO_CORRELATION_ID'",
    "eventIdExpression": "#randomUUID()",
    "timestampExpression": "#nowMillis()",
    "topicName" : "grnry_data_in_snowplow-webtracking",
    "retentionMs": 3456000000,
    "partitionCount": 32,
    "replication": 2,
    "eventstoreTTL": "P100Y",
    "description": "",
    "_links": {
        "self": {
            "href": "https://hostname/event-types/snowplow-webtracking/1"
        }
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If the version is not latest or a valid number greater or equals to 1.
{% endapi-method-response-example-description %}

```text
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'version' must be a natural number or 'latest' but is '-1'.",
    "details":"uri=/event-types/test-event-type/invalidVersion"
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
    "details": "uri=/event-types/adobe-s3/latest"
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
    "details":"uri=/event-types/test-event-type-partial/latest"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
No event type with given name \(case sensitive\) found for given version.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Event Type 'test-event-type' with version '4' not found.",
    "details":"uri=/event-types/test-event-type/4"
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
This request requires the role matching either `consumer` or `editor`. If `consumer` is null, any authenticated user is authorized. 
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
{% api-method-parameter name="export" type="string" required=false %}
if set to `"true"` \(not case sensitive\), the persister response will only contain properties that are not set to default values or generated by the api. Default is `""`.
{% endapi-method-parameter %}

{% api-method-parameter name="expand" type="string" required=false %}
Expand the response with `state` to show the persister state. Default is `""`.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
requested :event-type-name \(eventtype-1\) , :event-store-name \(pg\)
{% endapi-method-response-example-description %}

```text
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

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/event-types/adobe-s3/eventstores/pg/persister"
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
    "details":"uri=/event-types/test-event-type-partial/eventstores/pg/persister"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Event store, event type not found.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Persister 'adobe-s3' not found.",
    "details": "uri=/event-types/adobe-s3/eventstores/pgd/persister"
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
This request requires the role matching `editor`.
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

```text
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

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid parameters
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'event_type_name' cannot be null",
    "details":"uri=/event-types/event-type-empty-update-persister-config/eventstores/pg/persister"
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
    "details": "uri=/event-types/adobe-s3/eventstores/pg/persister"
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
    "details":"uri=/event-types/test-event-type-partial/eventstores/pg/persister"
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
This request requires a role matching `editor` or `consumer`. If `consumer` is null, any authenticated user is authorized.
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

{% endapi-method-response-example-description %}

```text
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

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/event-types/adobe-s3/eventstores/pg/persister/state"
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
    "details":"uri=/event-types/test-event-type-partial/eventstores/pg/persister/state"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Event store, event type not found.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Persister 'adobe-s3' not found.",
    "details": "uri=/event-types/adobe-s3/eventstores/pdg/persister/state"
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
Start/Stop Persister for a Specific Event Type
{% endapi-method-summary %}

{% api-method-description %}
Start or stop the state of a persister for an event type.  
This request requires the role matching `editor`.
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

```text
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

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/event-types/adobe-s3/eventstores/pg/persister/state"
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
    "details":"uri=/event-types/test-event-type-partial/eventstores/pg/persister/state"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Event store, event type not found.
{% endapi-method-response-example-description %}

```
{
     "timestamp": 1587303625038,
     "type": "entity_not_found",
     "message": "Persister 'adobe-s3' not found.",
     "details": "uri=/event-types/adobe-s3/eventstores/pdg/persister/logs"
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
This request requires the role matching `editor` or `consumer`. if `consumer` is null, any authenticated user is authorized.
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
Valid value are : `1 .. 500`. Default: `500`.
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
