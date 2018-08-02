# API Reference

{% api-method method="post" host="https://api.grnry.io" path="/dev-v1" %}
{% api-method-summary %}
Collect Events to Update a Profile
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Cookie" type="string" required=false %}
Updates a third-party user tracking cookie called "sp" for the Domain Name grnry.io, and returns the pixel to the client. The API will create a new Cookie as part of the response if no Cookie is set in the response.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="p" type="string" required=false %}
The platform the app runs on \(web, mob, app\)
{% endapi-method-parameter %}

{% api-method-parameter name="aid" type="string" required=false %}
Unique identifier for website / application
{% endapi-method-parameter %}

{% api-method-parameter name="tna" type="string" required=false %}
The Tracker Namespace parameter is used to distinguish between different trackers. The name can be any string that _does not_ contain a colon or semi-colon character. Tracker namespacing allows you to run multiple trackers, pinging to different collectors.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
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

{% api-method method="get" host="https://api.grnry.io" path="/dev-v1/profiles" %}
{% api-method-summary %}
Query Profiles
{% endapi-method-summary %}

{% api-method-description %}
retrieves a list of correlate IDs for profiles that match the query pattern
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="query" type="string" required=false %}
try "recent" to receive a list of the Correlate IDs for profiles that have been updated within the last 10 minutes
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
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

{% api-method method="get" host="https://api.grnry.io" path="/dev-v1/profiles/:id" %}
{% api-method-summary %}
Get Specific Profile
{% endapi-method-summary %}

{% api-method-description %}
Retrieve a full dump of the profile.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
ID of the profile.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token to enforce RBAC.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="pattern" type="string" %}
Limit the response to attribute that match the pattern string.
{% endapi-method-parameter %}

{% api-method-parameter name="expand" type="boolean" %}
Dereference linked profiles.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Cake successfully retrieved.
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
Could not find a cake matching this query.
{% endapi-method-response-example-description %}

```javascript
{
    "message": "Ain't no cake like that."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



