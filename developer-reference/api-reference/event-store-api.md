---
description: Springboot-based microservice to expose event data stored in the Event Store.
---

# Event Store API

## Paths

* GET /events/{correlationId}
* GET /events/{correlationId}/{eventId}
* GET /events

## API Methods

Consult the [Granary Access Clients Reference](../../operator-reference/identity-and-access-management/granary-access-clients.md#event-api-a-k-a-event-store-api) for roles a user needs to interact with Event Store API.

{% swagger baseUrl="https://api.grnry.io" path="/events/:correlationId" method="get" summary="Get Events by Correlation ID" %}
{% swagger-description %}
Retrieves events containing the specified Correlation ID. In order to get results, you must have the required roles as defined in the fields 

_event_type _

and 

_event_harvester_

. Otherwise, you will not get back any results.

\




\


Example:

\




\




`https://api.grnry.io/events/cookie123?from=1970-01-01T00:00:00Z&to=2038-01-01T00:00:00Z&offset=0&pagesize=20`

\



{% endswagger-description %}

{% swagger-parameter in="path" name="correlationId" type="string" required="true" %}
The correlation ID
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token
{% endswagger-parameter %}

{% swagger-parameter in="query" name="type" type="string" %}
event type associated with this correlation ID. 
{% endswagger-parameter %}

{% swagger-parameter in="query" name="expand" type="string" %}
Expand elements (detailed information). Comma-separated list of fields to include in response. Fields are null otherwise.  Valid values are "

**message**

" and "

**totalCount**

".
{% endswagger-parameter %}

{% swagger-parameter in="query" name="from (included)" type="string" %}
Timestamp encoded in ISO notation yyyy-MM-dd'T'hh:MM:ss'Z'. Defaults to 

**1970-01-01T00:00:00Z**
{% endswagger-parameter %}

{% swagger-parameter in="query" name="to (excluded)" type="string" %}
Timestamp encoded in ISO notation yyyy-MM-dd'T'hh:MM:ss'Z'. Defaults to 

**now()**
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="string" %}
Number of elements to be skipped. Defaults to 

**0**
{% endswagger-parameter %}

{% swagger-parameter in="query" name="pagesize" type="string" %}
Number of elements to be shown. Defaults to 

**20**
{% endswagger-parameter %}

{% swagger-response status="200" description="Retrieves the first 20 events (sorted by ascending timestamps) of a given correlation ID. Include the total count and the actual event message payload directly in the response by leveraging "expand".

Example: 

/events/cookie123?from=2017-01-01T00:00:00Z&to=2018-01-01T00:00:00Z&expand=totalCount,message
" %}
```javascript
{  
   "totalCount": 245,
   "events":[  
      {  
         "eventId":"event0815",
         "correlationId":"cookie123",
         "eventType":"web",
         "eventTypeVersion": 1,
         "eventHarvester":"testsource",
         "message": {
			 "customer_data" : {
				 "first_name" : "Max",
				 "last_name" : "Mustermann"
			 }
		 },
		 "created":"2018-11-01T11:47:38.000Z"
         "_links":{  
            "self":{  
               "href":"https://hostname/events/cookie123/event0815"
            }
         }
      },
     ...
      {  
         "eventId":"event0817",
         "correlationId":"cookie123",
         "eventType":"web",
         "eventTypeVersion": 1,
         "eventHarvester":"testsource",
         "message": {
			 "access_log" : {
				 "access_time" : "2018-11-01T11:47:59.000Z",
				 "access_url" : "http://www.website.com/index.html"
			 }
		 },
		 "created":"2018-11-01T11:47:59.000Z"
         "_links":{  
            "self":{  
               "href":"https://hostname/events/cookie123/event0817"
            }
         }
      }
   ],
   "_links":{  
      "self":{  
         "href":"https://playground.lce.grnry.io/events/cookie123?from=1970-01-01T00:00:00Z&to=2038-01-01T00:00:00Z&expand=false&offset=0&pagesize=20"
      }
   }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Returned in case of invalid parameter(s).
" %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'correlationId' must not be null.",
    "details": "uri=/events"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Access denied. Invalid or no token submitted." %}
```
{
    "timestamp": 1587302499600,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/events"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Returned when a user is not authorized to retrieve these events." %}
```
{
    "timestamp": 1587302499600,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/events"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Returned when no event was found, along with empty result set. 
" %}
```
{
    "timestamp": 1587302499600,
    "type": "entity_not_found",
    "message": "Events with correlationId 'cookie123' not found.",
    "details": "uri=/events"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/events/:correlationId/:eventId" method="get" summary="Get a Specific Event by ID and Correlation ID" %}
{% swagger-description %}
Retrieves a single event (including its message payload). In order to get results, you must have the required roles as defined in the fields 

_event_type _

and 

_event_harvester_

. Otherwise, you will not get back any results.
{% endswagger-description %}

{% swagger-parameter in="path" name="correlationId" type="string" required="true" %}
The correlation ID
{% endswagger-parameter %}

{% swagger-parameter in="path" name="eventId" type="string" required="true" %}
The event ID
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token
{% endswagger-parameter %}

{% swagger-response status="200" description="Retrieves an event in the scope of correlation ID "cookie123" and id "event0815".

Example: /events/cookie123/event0815
" %}
```
{
	"eventId":"event0815",
	"message":{
		...
		...
		},
	"created": "2019-03-21T11:54:55.234Z",
	"_links":{
		"self":{
			"href":"https://hostname/events/cookie123/event0815"
			}
		}
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Returned in case of invalid parameter(s).
" %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'correlationId' must not be null.",
    "details": "uri=/events"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Access denied. Invalid or no token submitted." %}
```
{
    "timestamp": 1587302499600,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/events"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Returned when the request to retrieve the events is unauthorized." %}
```
{
    "timestamp": 1587302499600,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/events"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="Returned when the requested object cannot be found.
" %}
```
{
    "timestamp": 1587302499600,
    "type": "entity_not_found",
    "message": "Event with correlationId 'cookie123' not found.",
    "details": "uri=/events"
}
```
{% endswagger-response %}
{% endswagger %}
