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
Profile Types successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
    "totalCount": 10,
    "profileTypes": [
        {
            "type": "_interaction",
            "_links": {
                "self": {
                    "href": "https://hostname/profiles/_contract?offset=0&pagesize=5"
                }
            }
        },
        {
            "type": "_claim",
            "_links": {
                "self": {
                    "href": "https://hostname/profiles/_claim?offset=0&pagesize=5"
                }
            }
        },
        {
            "type": "_contract",
            "_links": {
                "self": {
                    "href": "https://hostname/profiles/_contract?offset=0&pagesize=5"
                }
            }
        },
        {
            "type": "_customer",
            "_links": {
                "self": {
                    "href": "https://hostname/profiles/_customer?offset=0&pagesize=5"
                }
            }
        }
    ],
    "_links": {
        "next": {
            "href": "https://hostname/profiles/?offset=5&pagesize=5"
        }
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
A call for all profiles of type "\_contract" with page size 5:  
  
https://hostname/profiles/\_contract?pagesize=5
{% endapi-method-response-example-description %}

```javascript
{
    "totalCount": 3737,
    "profiles": [
        {
            "correlationId": "Contract47583",
            "type": "_contract",
            "jsonPayload": {
                "product": {
                    "_latest": {
                        "_c": 1,
                        "_v": "car fully comprehensive insurance",
                        "_in": 1585747023110,
                        "_ttl": "P100Y",
                        "_origin": "/belts/28",
                        "_reader": "_auth"
                    }
                }
            },
            "_links": {
                "self": {
                    "href": "https://hostname/profiles/_contract/Contract47583?fragments="
                }
            }
        },
        {
            "correlationId": "Contract47587",
            "type": "_contract",
            "jsonPayload": {
                "customer": {
                    "_latest": {
                        "_c": 1,
                        "_v": "83843",
                        "_in": 1586421719687,
                        "_ttl": "P100Y",
                        "_origin": "/belts/28",
                        "_reader": "_auth"
                    }
                }
            },
            "_links": {
                "self": {
                    "href": "https://hostname/profiles/_contract/Contract47587?fragments="
                }
            }
        },
        {
            "correlationId": "Contract80168",
            "type": "_contract",
            "jsonPayload": {
                "date": {
                    "next_payment": {
                        "_latest": {
                            "_c": 1,
                            "_v": "2020-01-01T00:00:00",
                            "_in": 1586421718311,
                            "_ttl": "P100Y",
                            "_origin": "/belts/28",
                            "_reader": "_auth"
                        }
                    }
                }
            },
            "_links": {
                "self": {
                    "href": "https://hostname/profiles/_contract/Contract80168?fragments="
                }
            }
        },
        {
            "correlationId": "Contract96511",
            "type": "_contract",
            "jsonPayload": {
                "date": {
                    "enddate": {
                        "_latest": {
                            "_c": 1,
                            "_v": "None",
                            "_in": 1586421739949,
                            "_ttl": "P100Y",
                            "_origin": "/belts/28",
                            "_reader": "_auth"
                        }
                    }
                }
            },
            "_links": {
                "self": {
                    "href": "https://hostname/profiles/_contract/Contract96511?fragments="
                }
            }
        },
        {
            "correlationId": "Contract67199",
            "type": "_contract",
            "jsonPayload": {
                "date": {
                    "startdate": {
                        "_latest": {
                            "_c": 1,
                            "_v": "2019-01-01T00:00:00",
                            "_in": 1586421746446,
                            "_ttl": "P100Y",
                            "_origin": "/belts/28",
                            "_reader": "_auth"
                        }
                    }
                }
            },
            "_links": {
                "self": {
                    "href": "https://hostname/profiles/_contract/Contract67199?fragments="
                }
            }
        }
    ],
    "_links": {
        "self": {
            "href": "https://hostname/profiles/_contract?offset=0&pagesize=5"
        },
        "next": {
            "href": "https://hostname/profiles/_contract?offset=5&pagesize=5"
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

{% hint style="warning" %}
Profiles and their grains in the JSON response body are unordered. 
{% endhint %}

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
Filters the profile by grain/fragment path\(s\). You can define multiple path as a comma-separated list.   
Example:  
`/customer/name,/coustomer/address,/invoiceDetails`
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A call for a specific profile, given profile type "\_interaction" and correlationId "Session56202":  
  
https://hostname/profiles/\_interaction/Session56202
{% endapi-method-response-example-description %}

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

{% hint style="warning" %}
Profiles and their grains in the JSON response body are unordered. 
{% endhint %}

