---
description: Harvester API's event type endpoints for Live-Segment Persisters
---

# Live-Segment Persisters

## Paths

* GET /projects/{project-name}/event-types/{event-type-name}/eventstores/live-segment/persister
* PUT /projects/{project-name}/event-types/{event-type-name}/eventstores/live-segment/persister
* GET /projects/{project-name}/event-types/{event-type-name}/eventstores/live-segment/persister/state
* POST /projects/{project-name}/event-types/{event-type-name}/eventstores/live-segment/persister/state

## API Methods

Consult the [Granary Access Clients Reference](../../../../operator-reference/identity-and-access-management/granary-access-clients.md#harvester-api) for roles a user needs to interact with Harvester API.



{% swagger baseUrl="https://api.grnry.io" path="/projects/:project-name/event-types/:event-type-name:/eventstores/live-segment/persister" method="get" summary="Get Persister Configuration" %}
{% swagger-description %}
Requires 

`editor`

 or 

`viewer`

 role.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" required="false" %}
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

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="expand" type="string" %}
If set the status of the persister is appended to the the result
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" required="true" %}
Event type name.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```json
{
    "eventTypeName": "User-Visit",
    "eventstoreType": "live-segment",
    "tasks.max": "1",
    "batchSize": "10",
    "maxRetries": "3",
    "retryBackoffMs": "1000"
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Token invalid or missing." %}
```javascript
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/live-segment/persister"
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="Missing roles to access this resource." %}
```javascript
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type-partial/eventstores/live-segment/persister"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Resource not found." %}
```
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Persister 'adobe-s3' not found.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/live-segment/persister"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/:project-name/event-types/:event-type-name/eventstores/live-segment/persister" method="put" summary="Set Persister" %}
{% swagger-description %}
Requires 

`editor`

 role.
{% endswagger-description %}

{% swagger-parameter in="body" name="retryBackoffMs" type="string" %}
Time to wait until the next retry
{% endswagger-parameter %}

{% swagger-parameter in="body" name="maxRetries" type="string" %}
Defines the number of retries Kafka makes to insert data into the database
{% endswagger-parameter %}

{% swagger-parameter in="body" name="batchSize" type="string" %}
Specifies how many sets of data are inserted into the database table at the same time 
{% endswagger-parameter %}

{% swagger-parameter in="body" name="taskMax" type="number" %}
Sets the maximal number of task that can be scheduled by Kafka. This number can not exceed the number of partitions of the topic
{% endswagger-parameter %}

{% swagger-parameter in="path" name="project-name" type="string" required="false" %}
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
Event type name.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```json
{
    "eventTypeName": "User-Visit",
    "eventstoreType": "live-segment",
    "tasks.max": "2",
    "batchSize": "10",
    "maxRetries": "3",
    "retryBackoffMs": "1000"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Invalid parameters." %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Token invalid or missing." %}
```javascript
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/live-segment/persister"
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="Missing roles to manipulate this resource." %}
```javascript
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type-partial/eventstores/live-segment/persister"
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="" %}
```javascript
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Persister 'adobe-s3' not found.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/live-segment/persister"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/:project-name/event-types/:event-type-name/eventstores/live-segment/persister/state" method="get" summary="Get State of Persister" %}
{% swagger-description %}
Requires 

`editor`

 or 

`viewer`

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
Event type name.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```json
{
    "status": "STOPPED",
    "_links": {
        "self": {
            "href": "https://hostname/projects/global/event-types/user-visits/eventstores/live-segment/persister/state"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Token invalid or missing." %}
```javascript
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/live-segment/persister/state"
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="Missing roles to access this resource." %}
```javascript
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type-partial/eventstores/live-segment/persister/state"
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="" %}
```javascript
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Persister 'adobe-s3' not found.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/live-segment/persister/state"
}
```
{% endswagger-response %}
{% endswagger %}

Possible values for the status attribute in the response body are:

| STOPPED   | persister is not deployed             |
| --------- | ------------------------------------- |
| DEPLOYING | persister is being deployed           |
| RUNNING   | persister is running                  |
| FAILED    | persister is deployed but not running |

{% swagger baseUrl="https://api.grnry.io" path="/projects/:project-name/event-types/:event-type-name/eventstores/live-segment/persister/state" method="post" summary="Set State of Persister" %}
{% swagger-description %}
Requires 

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

{% swagger-parameter in="body" name="action" type="string" required="true" %}
Describes the desired state. Possible values: START, STOP, RESTART
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event-type-name" type="string" required="true" %}
Event type name.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    "timestamp": 1634736705721,
    "message": "Parameter 'Action' Parameter can only have value of START,STOP,RESTART.",
    "type": "bad_parameter_value",
    "traceId": "2343148326994162096",
    "invalidParams": [
        {
            "type": "Parameter",
            "name": "Action",
            "reason": "Parameter can only have value of START,STOP,RESTART"
        }
    ],
    "details": "uri=/event-types/User-Visits-5/eventstores/live-segment/persister/state"
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Token missing or invalid." %}
```javascript
{
    "timestamp": 1587302709982,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/live-segment/persister/state"
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="Missing roles to manipulate this resource." %}
```javascript
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/projects/global/event-types/test-event-type-partial/eventstores/live-segment/persister/state"
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="" %}
```javascript
{
    "timestamp": 1587303625038,
    "type": "entity_not_found",
    "message": "Persister 'adobe-s3' not found.",
    "details": "uri=/projects/global/event-types/adobe-s3/eventstores/live-segment/persister/state"
}
```
{% endswagger-response %}
{% endswagger %}
