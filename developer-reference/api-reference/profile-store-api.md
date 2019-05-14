---
description: >-
  Springboot-based microservice to expose profile data stored in the Profile
  Store.
---

# Profile Store API

## Paths

* /profiles/{correlationId}
* /profiles/{profileType}/{correlationId}
* /profiles/type/{profileType}

## API Methods

{% api-method method="get" host="https://api.grnry.io" path="/profiles/:correlationId" %}
{% api-method-summary %}
Get a Specific Profile
{% endapi-method-summary %}

{% api-method-description %}
Retrieves a full dump of a profile based on default profile type.
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
    "type": "_d",
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

{% api-method method="get" host="https://api.grnry.io" path="/profiles/:profileType/:correlationId" %}
{% api-method-summary %}
Get a Specific Profile by ID and Type
{% endapi-method-summary %}

{% api-method-description %}
Retrieves a full dump of a profile fetched by ID and profile type.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="profileType" type="string" required=true %}
The profile type
{% endapi-method-parameter %}

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

{% endapi-method-response-example-description %}

```
{
    "correlationId": "71f48a4b-c8f3-4937-9691-442384c565d8",
    "type": "someType",
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

{% api-method method="get" host="https://api.grnry.io" path="/profiles/type/:profileType" %}
{% api-method-summary %}
Get all Profiles of a Specific Type
{% endapi-method-summary %}

{% api-method-description %}
Retrieves a full dump of all profiles filtered by profile type.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="profileType" type="string" required=true %}
The profile type
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
  "totalCount": 2,
  "profiles": [
    {
      "correlationId": "15d0f494-2afd-4d79-9fe4-ce6a4add4e2d",
      "type": "someProfileType",
      "jsonPayload": {
        "contentType": {
          "_latest": {
            "_c": 0.4,
            "_v": "text/html",
            "_in": 1539358945000,
            "_origin": "myorigin",
            "_reader": "_all",
            "_ttl": "P100Y"
          }
        }
      }
    },
    {
      "correlationId": "15d0f494-2afd-4d79-9fe4-ce6a4add4e33",
      "type": "someProfileType",
      "jsonPayload": {
        "contentType": {
          "_latest": {
            "_c": 0.4,
            "_v": "text/html",
            "_in": 1539358945000,
            "_origin": "myorigin",
            "_reader": "_all",
            "_ttl": "P100Y"
          }
        }
      }
    }
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
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



