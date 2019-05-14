---
description: >-
  Springboot-based microservice to expose profile data stored in the Profile
  Store.
---

# Profile Store API

## Paths

* /profiles/{correlationId}

## API Methods

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
The correlation ID
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
Profile successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
    "correlationId": "71f48a4b-c8f3-4937-9691-442384c565d8",
    "jsonPayload": {
        "device": {
            "device_type": {
                "_latest": {
                    "_c": 1,
                    "_v": "unknown",
                    "_in": 1552328425169,
                    "_origin": "belt-appsprung-web-event",
                    "_reader": "_all",
                    "_ttl": "P100Y"
                }
            },
            "operating_system": {
                "_latest": {
                    "_c": 1,
                    "_v": "Mac OS",
                    "_in": 1552328425177,
                    "_origin": "belt-appsprung-web-event",
                    "_reader": "_all",
                    "_ttl": "P100Y"
                }
            }
        },
        
        ...
        ...
        
        "_id": "71f48a4b-c8f3-4937-9691-442384c565d8"
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Access denied. Invalid token submitted.
{% endapi-method-response-example-description %}

```

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





