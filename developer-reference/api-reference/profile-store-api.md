---
description: >-
  Springboot-based microservice to expose profile data stored in the Profile
  Store.
---

# Profile Store API

## Paths

* /profiles/{profileType}/{correlationId}
*   /profiles/{profileType}/{correlationId}/grain/{path}



## API Methods

Consult the [Granary Access Clients Reference](../../operator-reference/identity-and-access-management/granary-access-clients.md#profile-api-a-k-a-profile-store-api) for roles a user needs to interact with Profile Store API.

{% swagger baseUrl="https://api.grnry.io" path="/profiles/:profileType/:correlationId" method="get" summary="Get a Specific Profile by ID and Type" expanded="true" %}
{% swagger-description %}
Retrieves a full dump of a profile fetched by ID and profile type.

\




\


In order to get results, you must have the required roles as defined in the field 

_reader_

. Otherwise, you will not get back any results.
{% endswagger-description %}

{% swagger-parameter in="path" name="profileType" type="string" %}
The profile type.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="correlationId" type="string" %}
The correlation ID.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="withHistory" type="boolean" %}
If 

`withHistory`

 is 

`false`

 only the latest version of grains are returned. If 

`withHistory`

 is 

`true`

 of all grains all version are returned. Defaults to 

`true`

.

\


Only available starting from Profilestore API version 1.0.1.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="fragments" type="string" %}
Filters the profile by grain/fragment path(s). You can define multiple path as a comma-separated list. Example: 

`/customer/name,/customer/adress,/invoiceDetails`
{% endswagger-parameter %}

{% swagger-parameter in="query" name="resolveProfileLinks" type="boolean" %}
If 

`resolveProfileLinks`

 is 

`true`

 first the DB will be queried to find a ProfileLink for the input 

`correlationId`

. If found, the Profile will be returned for the Linked Correlation Id, otherwise the Profile for the input 

`correlationId`

 will be returned
{% endswagger-parameter %}

{% swagger-response status="200" description="A call for a specific profile, given profile type "_interaction" and correlationId "Session56202":

https://hostname/profiles/_interaction/Session56202" %}
```javascript
{
    "correlationId": "Session56202",
    "type": "_interaction",
    "jsonPayload": {
        "date": {
            "end": {
                "_latest": {
                    "_c": 1,
                    "_v": "2018-05-14T14:15:42",
                    "_in": 1587644709930,
                    "_ttl": "P100Y",
                    "_origin": "/belts/28",
                    "_reader": "_auth"
                }
            },
            "start": {
                "_latest": {
                    "_c": 1,
                    "_v": "2018-05-14T14:05:56",
                    "_in": 1587644709898,
                    "_ttl": "P100Y",
                    "_origin": "/belts/28",
                    "_reader": "_auth"
                }
            }
        },
        "_id@_contract@profilestore": {
            "_latest": {
                "_c": 1,
                "_v": [
                    "Contract56242"
                ],
                "_in": 1587644709853,
                "_ttl": "P100Y",
                "_origin": "/belts/28",
                "_reader": "_pers"
            }
        },
        "_id@_customer@profilestore": {
            "_latest": {
                "_c": 1,
                "_v": [
                    "Customer65882"
                ],
                "_in": 1587644709827,
                "_ttl": "P100Y",
                "_origin": "/belts/28",
                "_reader": "_pers"
            }
        },
        "process": {
            "context": {
                "_latest": {
                    "_c": 1,
                    "_v": "size:56sq,value:60.000£",
                    "_in": 1587644709773,
                    "_ttl": "P100Y",
                    "_origin": "/belts/28",
                    "_reader": "_pers"
                }
            },
            "counter": {
                "_latest": {
                    "_c": 1,
                    "_v": {
                        "_step": 1.0,
                        "_current": 5,
                        "_initial": 0.0
                    },
                    "_in": 1587644710593,
                    "_ttl": "P100Y",
                    "_origin": "/belts/28",
                    "_reader": "_auth"
                }
            },
            "outcome": {
                "_latest": {
                    "_c": 1,
                    "_v": "contract",
                    "_in": 1587644711294,
                    "_ttl": "P100Y",
                    "_origin": "/belts/28",
                    "_reader": "_auth"
                }
            },
            "product": {
                "_latest": {
                    "_c": 1,
                    "_v": "home contents insurance",
                    "_in": 1587644711071,
                    "_ttl": "P100Y",
                    "_origin": "/belts/28",
                    "_reader": "_auth"
                }
            },
            "type": {
                "_latest": {
                    "_c": 1,
                    "_v": "calculate quote",
                    "_in": 1587644710573,
                    "_ttl": "P100Y",
                    "_origin": "/belts/28",
                    "_reader": "_auth"
                }
            }
        },
        "_id": "Session56202"
    }
}
```
{% endswagger-response %}

{% swagger-response status="401" description="" %}
```
{
    "timestamp": 1587302499600,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/profiles"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="" %}
```
{
    "timestamp": 1587302499600,
    "type": "entity_not_found",
    "message": "Profile with correlationId 'Session56202' not found.",
    "details": "uri=/profiles"
}
```
{% endswagger-response %}
{% endswagger %}

{% hint style="warning" %}
Profiles and their grains in JSON response body are unordered.
{% endhint %}



{% swagger baseUrl="https://api.grnry.io" path="/profiles/:profileType/:correlationId/grain/:path" method="get" summary="Get a Specific Grain by ID, Type and Path" %}
{% swagger-description %}
Allows retrieval of grain for latest point in time or of all grain versions paginated depending on the query parameter 

`withHistory`

.

\




\


In order to get results, you must have the required roles as defined in the field 

_reader_

. Otherwise, you will not get back any results.

\



{% endswagger-description %}

{% swagger-parameter in="path" name="path" type="string" %}
The full path related to the grain.

\


Example: 

`customer/name`
{% endswagger-parameter %}

{% swagger-parameter in="path" name="profileType" type="string" %}
The profile type.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="correlationId" type="string" %}
The correlation ID.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="pageSize" type="integer" %}
Determines the amount of grains returned per page. The maximum is 250.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="integer" %}
Allows retrieval of historic grains not contained in the first page.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="withHistory" type="boolean" %}
Determines if for the grain only the latest version is returned or all version paginated.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="resolveProfileLinks" type="boolean" %}
If 

`resolveProfileLinks`

 is 

`true`

 first the DB will be queried to find a ProfileLink for the input 

`correlationId`

. If found, the Profile will be returned for the Linked Correlation Id, otherwise the Profile for the input 

`correlationId`

 will be returned
{% endswagger-parameter %}

{% swagger-response status="200" description="A call for multiple versions of a specific grain with given profile type "grainProfile" and correlationId "28072021" and the path "customer/contract".

https://hostname/profiles/grainProfile/28072021/grain/customer/contract?pageSize=2&offset=0&withHistory=True" %}
```
{
    "correlationId": "28072021",
    "type": "grainProfile",
    "jsonPayload": {
        "customer": {
            "contract": {
                "_latest": {
                    "_c": 0.5,
                    "_v": "Customer changed insurance contract.",
                    "_in": 1539358945000,
                    "_ttl": "P100Y",
                    "_ttn": "P100Y",
                    "_origin": "myorigin",
                    "_reader": "_all"
                },
                "_20210706T155637.111Z": {
                    "_c": 0.5,
                    "_v": "Customer made insurance contract with Company",
                    "_in": 1539358945000,
                    "_ttl": "P100Y",
                    "_ttn": "P100Y",
                    "_origin": "myorigin",
                    "_reader": "_all"
                }
            }
        },
        "_id": "28072021"
    },
    "totalGrainVersions": 2,
    "_links": {
        "self": {
            "href": "https://teststage.internal.analytics.cc.syncier.cloud/profiles/grainProfile/28072021/grain/customer/contract?pageSize=2&offset=0&withHistory=True"
        },
        "next": {
            "href": "https://teststage.internal.analytics.cc.syncier.cloud/profiles/grainProfile/28072021/grain/customer/contract?pageSize=2&offset=2&withHistory=True"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="" %}
```
{
    "timestamp": "2021-07-28T15:58:11.143+0000",
    "message": "Parameter 'path' You need to provide the full path of the grain.",
    "type": "bad_parameter_value",
    "traceId": "5184097694623809509",
    "invalidParams": [
        {
            "name": "path",
            "reason": "You need to provide the full path of the grain"
        }
    ],
    "details": "uri=/profiles/grainProfile/28072021/grain/"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="" %}
```
{
    "timestamp": 1587302499600,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/profiles/grainProfile/28072021999/grain/customer/contract"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="" %}
```
{
    "timestamp": "2021-07-28T15:45:08.230+0000",
    "message": "Grains with profileType 'grainProfile' and correlationId '28072021999' not found.",
    "type": "entity_not_found",
    "traceId": "-2256169692750680503",
    "details": "uri=/profiles/grainProfile/28072021999/grain/customer/contract"
}
```
{% endswagger-response %}
{% endswagger %}

{% hint style="warning" %}
Only available starting from Profilestore API version 1.0.1.
{% endhint %}
