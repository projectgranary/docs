# API Reference

## **Authentication**

All REST API endpoints of GRNRY are protected via **OpenID Connect** \(OIDC\) which is based on **OAuth 2.0**. Therefore you need a **JSON Web Token** \(JWT [RFC 7519](https://tools.ietf.org/html/rfc7519)\) to access the API. You can find your Authentication Server at the `/auth` path under your GRNRY domain. For example `demo.grnry.io`**`/auth`**

The Authorization header field needs to be send with every request. Since the JWT is used as a bearer token \([RFC 6750](https://tools.ietf.org/html/rfc6750)\) the header field looks like follows:

```text
Authorization: Bearer <token>
```

## Event Store API

Springboot-based microservice to expose event data stored in the [Eventstore](dataflow/event-store.md). 

### Configuration

{% tabs %}
{% tab title="Spec" %}
| Environment Variable | Value | Description |
| :--- | :--- | :--- |
| JAVA\_OPTS | Xms512m -Xmx512m | JVM specific configuration like head-size, GC settings, ... |
| SPRING\_PROFILE | PROD | Not used at the moment, provide a dummy value like 'PROD' |
| EVENTSTORE\_DB\_HOSTNAME | grnry-pg-citus-0.grnry-pg-citus | Hostname of the Postgres instance |
| EVENTSTORE\_DB\_PORT | 5432 | Port of the Postgres instance |
| EVENTSTORE\_DB\_DATABASE | postgres | Database of the Postgres instance |
| EVENSTORE\_DB\_USERNAME | username | Username of the Postgres instance |
| EVENSTORE\_DB\_PASSWORD | password | Password of the Postgres instance |
| EVENTSTORE\_DB\_SSL | false | If database connection should use ssl |
| EVENTSTORE\_DB\_SSL\_VALIDATE | false | If server certificate should be validated at secure database connection |
{% endtab %}
{% endtabs %}

### Paths

* /events/{correlationId}
* /events/{correlationId}/{eventId}

### API Methods

{% api-method method="get" host="https://api.grnry.io" path="/events/:correlationId" %}
{% api-method-summary %}
Get events by correlation ID
{% endapi-method-summary %}

{% api-method-description %}
Retrieves events by their correlation id.  
  
Example:  
  
`https://playground.lce.grnry.io/events/cookie123?from=1970-01-01T00:00:00Z&to=2038-01-01T00:00:00Z&expand=false&offset=0&pagesize=20`  
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

```
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

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Returned when no event was found, along with empty result set.   
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=405 %}
{% api-method-response-example-description %}
Returned when a user is not authorized to retrieve these events.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=503 %}
{% api-method-response-example-description %}
Returned in case of invalid parameter\(s\).  
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/events/:correlationId/:eventId" %}
{% api-method-summary %}
Get a specific event by ID and correlation ID
{% endapi-method-summary %}

{% api-method-description %}
Retrieves a single event \(including actual message payload\).
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
	"_links":{
		"self":{
			"href":"https://playground.lce.grnry.io/events/cookie123/event0815"
			}
		}
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Returned when the requested object cannot be found.  
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=405 %}
{% api-method-response-example-description %}
Returned when the request to retrieve the events is unauthorized.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=503 %}
{% api-method-response-example-description %}
Returned in case of invalid parameter\(s\).  
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Profile Store API

Springboot-based microservice to expose profile data stored in the [Profile Store](dataflow/profile-store.md).

### Configuration

{% tabs %}
{% tab title="Spec" %}
| Environment Variable | Value | Description |
| :--- | :--- | :--- |
| JAVA\_OPTS | Xms512m -Xmx512m | JVM specific configuration like head-size, GC settings, ... |
| SPRING\_PROFILE | PROD | Not used at the moment, provide a dummy value like 'PROD' |
| PROFILESTORE\_DB\_HOSTNAME | grnry-pg-citus-0.grnry-pg-citus | Hostname of the Postgres instance |
| PROFILESTORE\_DB\_PORT | 5432 | Port of the Postgres instance |
| PROFILESTORE\_DB\_DATABASE | postgres | Database of the Postgres instance |
| PROFILESTORE\_DB\_USERNAME | username | Username of the Postgres instance |
| PROFILESTORE\_DB\_PASSWORD | password | Password of the Postgres instance |
| PROFILESTORE\_DB\_SSL | false | If database connection should use ssl |
| PROFILESTORE\_DB\_SSL\_VALIDATE | false | If server certificate should be validated at secure database conn |
{% endtab %}
{% endtabs %}

### Paths

* /profiles/{profilesId}

### API Methods

{% api-method method="get" host="https://api.grnry.io" path="/profiles/:correlationId" %}
{% api-method-summary %}
Get A Specific Profile
{% endapi-method-summary %}

{% api-method-description %}
Retrieves a full dump of the profile.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="correlationId" type="string" required=true %}
ID of the profile.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token to enforce RBAC.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Profile successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
    "id": "693869837493493",
    "age": "57",
    "versions": [
        "ts": "2018-01-01",
        "age": "56"
        ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find a profile matching this query.
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Snowplow API endpoints

If you are using a [tracker](https://github.com/snowplow/snowplow/wiki/trackers) from the Snowplow project you can follow [this instructions](https://docs.grnry.io/~/edit/drafts/-LUjJ4toeH9b8wlj-_8p/user-guide/using-the-javascript-tracker).

Analog to the Snowplow trackers, the Snowplow API accepts both GET and POST requests and is located at **`/api/com.snowplowanalytics.snowplow/tp2`** . Upon receipt, the API **could** set/update a third-party cookie to allow cross-domains user tracking.  The ID in this third-party user tracking cookie would be then stored in the `network_userid` field in Snowplow events. To activate this, set "crossDomain" and/or "cookie" fields to ENABLED in the collector [configuration file](https://gitlab.alvary.io/grnry/deployment/blob/master/charts/incubator/snowplow-scala-stream-collector/templates/configmap.yaml). Currently, they are both **disabled**.

GET requests to the API should have the events' parameters appended to the end of each request, while POST requests should include them in the request body. The selection of events' parameters is arbitrary. For a list of common parameters used by Snowplow tracker, please refer [here](https://github.com/snowplow/snowplow/wiki/snowplow-tracker-protocol).

{% api-method method="get" host="https://api.grnry.io" path="/api/com.snowplowanalytics.snowplow/tp2?param1=test&param2=test..." %}
{% api-method-summary %}
Tracker endpoint GET
{% endapi-method-summary %}

{% api-method-description %}
Events' parameters should be appended to the end of each request.  
  
Example:  
  
`https://api.grnry.io/api/com.snowplowanalytics.snowplow/tp2?f_java=1&aid=lcePrices&tv=js-0.26.1&e=pv&ds=1105x390&cookie=1`
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="user-agent" type="string" required=false %}
Agent firing this request
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

{% api-method method="post" host="https://api.grnry.io" path="/api/com.snowplowanalytics.snowplow/tp2" %}
{% api-method-summary %}
Tracker endpoint POST
{% endapi-method-summary %}

{% api-method-description %}
Events' parameters should be added into the body as String.  
  
Example JSON body:  
  
{"id" : "1234", "tsa": "test"}  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="user-agent" type="string" required=false %}
Agent firing this request
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

