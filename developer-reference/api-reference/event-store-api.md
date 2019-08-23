---
description: Springboot-based microservice to expose event data stored in the Event Store.
---

# Event Store API

## Paths

* /events/{correlationId}
* /events/{correlationId}/{eventId}

## API Methods

{% api-method method="get" host="https://api.grnry.io" path="/events/:correlationId" %}
{% api-method-summary %}
Get Events by Correlation ID
{% endapi-method-summary %}

{% api-method-description %}
Retrieves events containing the specified Correlation ID. In order to get results, you must have the required roles as defined in the fields _event\_type_ and _event\_harvester_. Otherwise, you will not get back any results.  
  
Example:  
  
`https://api.grnry.io/events/cookie123?from=1970-01-01T00:00:00Z&to=2038-01-01T00:00:00Z&expand=false&offset=0&pagesize=20`  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="correlationId" type="string" required=true %}
The correlation ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="expand" type="string" required=false %}
Expand elements \(detailed information\). Comma-separated list of fields to include in response. Fields are null otherwise.  Valid values are "**message**" and "**totalCount**".
{% endapi-method-parameter %}

{% api-method-parameter name="from \(included\)" type="string" required=false %}
Timestamp encoded in ISO notation yyyy-MM-dd'T'hh:MM:ss'Z'. Defaults to **1970-01-01T00:00:00Z**
{% endapi-method-parameter %}

{% api-method-parameter name="to \(excluded\)" type="string" required=false %}
Timestamp encoded in ISO notation yyyy-MM-dd'T'hh:MM:ss'Z'. Defaults to **2038-01-01T00:00:00Z**
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="string" required=false %}
Number of elements to be skipped. Defaults to **0**
{% endapi-method-parameter %}

{% api-method-parameter name="pagesize" type="string" required=false %}
Number of elements to be shown. Defaults to **20**
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Retrieves the first 20 events \(sorted by ascending timestamps\) of a given correlation ID. Include the total count and the actual event message payload directly in the response by leveraging "expand".  
  
Example:   
  
/events/cookie123?from=2017-01-01T00:00:00Z&to=2018-01-01T00:00:00Z&expand=totalCount,message  
{% endapi-method-response-example-description %}

```javascript
{  
   "totalCount": 245,
   "events":[  
      {  
         "eventId":"event0815",
         "message": {
			 "customer_data" : {
				 "first_name" : "Max",
				 "last_name" : "Mustermann"
			 }
		 }
		 "created":"2018-11-01T11:47:38.000Z"
         "_links":{  
            "self":{  
               "href":"https://playground.lce.grnry.io/events/cookie123/event0815"
            }
         }
      },
     ...
      {  
         "eventId":"event0817",
         "message": {
			 "access_log" : {
				 "access_time" : "2018-11-01T11:47:59.000Z",
				 "access_url" : "http://www.website.com/index.html"
			 }
		 }
		 "created":"2018-11-01T11:47:59.000Z"
         "_links":{  
            "self":{  
               "href":"https://playground.lce.grnry.io/events/cookie123/event0817"
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
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Returned in case of invalid parameter\(s\).  
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Access denied. Invalid or no token submitted.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Returned when a user is not authorized to retrieve these events.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Returned when no event was found, along with empty result set.   
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/events/:correlationId/:eventId" %}
{% api-method-summary %}
Get a Specific Event by ID and Correlation ID
{% endapi-method-summary %}

{% api-method-description %}
Retrieves a single event \(including its message payload\). In order to get results, you must have the required roles as defined in the fields _event\_type_ and _event\_harvester_. Otherwise, you will not get back any results.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="correlationId" type="string" required=true %}
The correlation ID
{% endapi-method-parameter %}

{% api-method-parameter name="eventId" type="string" required=true %}
The event ID
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
Retrieves an event in the scope of correlation ID "cookie123" and id "event0815".  
  
Example: /events/cookie123/event0815  
{% endapi-method-response-example-description %}

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
			"href":"https://playground.lce.grnry.io/events/cookie123/event0815"
			}
		}
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Returned in case of invalid parameter\(s\).  
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Access denied. Invalid or no token submitted.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Returned when the request to retrieve the events is unauthorized.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Returned when the requested object cannot be found.  
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## 

