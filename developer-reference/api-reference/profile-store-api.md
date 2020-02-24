---
description: >-
  Springboot-based microservice to expose profile data stored in the Profile
  Store.
---

# Profile Store API

## Paths

* /profiles
* /profiles/{profileType}
* /profiles/{profileType}/{correlationId}

## API Methods

Consult the [Granary Access Clients Reference](../../operator-reference/identity-and-access-management/granary-access-clients.md#profile-api-a-k-a-profile-store-api) for roles a user needs to interact with Profile Store API.

{% api-method method="get" host="https://api.grnry.io" path="/profiles" %}
{% api-method-summary %}
Get list of Profile Types
{% endapi-method-summary %}

{% api-method-description %}
Retrieves a list of profile types.  
  
In order to get results, you must have the required roles as defined in the field _reader_. Otherwise, you will not get back any results.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="pagesize" type="integer" required=false %}
Number of results to be returned. Default  is 20, max is 100
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="integer" required=false %}
Start offset. Default is 0. 
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Profile successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
  "totalCount": 2,
  "profileTypes": [
    {
      "type": "_d",
      "_links": {
        "self": {
          "href": "http://localhost/profiles/_d?offset=0&pagesize=20"
        }
      }
    },
    {
      "type": "someProfileType",
      "_links": {
        "self": {
          "href": "http://localhost/profiles/someProfileType?offset=0&pagesize=20"
        }
      }
    }
  ]
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
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/profiles/:profileType" %}
{% api-method-summary %}
Get all Profiles of a Specific Type
{% endapi-method-summary %}

{% api-method-description %}
Retrieves a full dump of all profiles filtered by profile type.  
  
In order to get results, you must have the required roles as defined in the field _reader_. Otherwise, you will not get back any results.
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

{% api-method-query-parameters %}
{% api-method-parameter name="pagesize" type="integer" required=false %}
Number of results to be returned. Default is 20, max is 100.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="integer" required=false %}
Start offset. Default is 0
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
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
            "_ttl": "P100Y",
            "_origin": "myorigin",
            "_reader": "_all"
          }
        },
        "contentType2": {
          "_latest": {
            "_c": 0.4,
            "_v": "text/html",
            "_in": 1539358945000,
            "_ttl": "P100Y",
            "_origin": "myorigin",
            "_reader": "_all"
          },
          "_nonlatest": {
            "_c": 0.5,
            "_v": "text/html",
            "_in": 1539358945001,
            "_ttl": "P100Y",
            "_origin": "myorigin",
            "_reader": "_all"
          },
          "test": {
            "path": {
              "_latest": {
                "_c": 0.4,
                "_v": "text/html",
                "_in": 1539358945000,
                "_ttl": "P100Y",
                "_origin": "myorigin",
                "_reader": "_all"
              }
            }
          }
        }
      },
      "_links": {
        "self": {
          "href": "http://localhost/profiles/someProfileType/15d0f494-2afd-4d79-9fe4-ce6a4add4e2d"
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
            "_ttl": "P100Y",
            "_origin": "myorigin",
            "_reader": "_all"
          }
        }
      },
      "_links": {
        "self": {
          "href": "http://localhost/profiles/someProfileType/15d0f494-2afd-4d79-9fe4-ce6a4add4e33"
        }
      }
    }
  ],
  "_links": {
    "self": {
      "href": "http://localhost/profiles/someProfileType?offset=0&pagesize=20"
    }
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
If given profile type is not found
{% endapi-method-response-example-description %}

```

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
  
In order to get results, you must have the required roles as defined in the field _reader_. Otherwise, you will not get back any results.
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

{% api-method-query-parameters %}
{% api-method-parameter name="fragments" type="string" required=false %}
Filters the profile by grain/fragment path\(s\). You can define multiple path as a comma-separated list. Example: `/customer/name,/customer/adress,/invoiceDetails`
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

