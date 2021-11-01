---
description: Harvester API's source type endpoints.
---

# Source Type Endpoints

## Paths

* GET /harvesters/source-types
* GET /harvesters/source-types/{source-type-name}
* GET /harvesters/source-types/{source-type-name}/{version}

## API Methods

Consult the [Granary Access Clients Reference](../../../operator-reference/identity-and-access-management/granary-access-clients.md#harvester-api) for roles a user needs to interact with Harvester API.

{% swagger baseUrl="https://api.grnry.io" path="/harvesters/source-types" method="get" summary="Get all Source Types" %}
{% swagger-description %}
Get latest version of all source-types.

\


This request requires the 

`viewer`

 or 

`editor`

 role in at least one project.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="search" type="string" %}
Filter source types by names containing this search term Default is 

`""`

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="pagesize" type="number" %}
Number of source types returned per page. Default is 

`20`

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="number" %}
Offset of the requested page. Default is 

`0`

. Must be a whole multiple of 

`pagesize`

. 
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "sourceTypes": [
        {
            "name": "grnry-adobe-sftp",
            "_links": {
                "self": {
                    "href": "https://hostname/harvesters/source-types/grnry-adobe-sftp?offset=0&pagesize=20&expand="
                }
            }
        },
        {
            "name": "grnry-http",
            "_links": {
                "self": {
                    "href": "https://hostname/harvesters/source-types/grnry-http?offset=0&pagesize=20&expand="
                }
            }
        },
        {
            "name": "grnry-jdbc",
            "_links": {
                "self": {
                    "href": "https://hostname/harvesters/source-types/grnry-jdbc?offset=0&pagesize=20&expand="
                }
            }
        },
        {
            "name": "grnry-sftp",
            "_links": {
                "self": {
                    "href": "https://hostname/harvesters/source-types/grnry-sftp?offset=0&pagesize=20&expand="
                }
            }
        }
    ],
    "totalCount": 4,
    "_links": {
        "self": {
            "href": "https://hostname/harvesters/source-types?search=&offset=0&pagesize=20&expand=totalCount"
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Invalid query parameter value." %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'pagesize' must be a natural number but is '-1'.",
    "details": "uri=/harvesters/source-types"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/source-types"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/harvesters/source-types"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/harvesters/source-types/:source-type-name" method="get" summary="Get all versions of an Source Type" %}
{% swagger-description %}
Get all versions of a given source type.

\


This request requires the 

`viewer`

 or 

`editor`

 role in at least one project.
{% endswagger-description %}

{% swagger-parameter in="path" name="source-type-name" type="string" required="true" %}
Name of the source type.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="pagesize" type="number" %}
Number of source types returned per page. Default is

\




`20`

 .
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="number" %}
Offset of the requested page. Default is 

`0`

 . Must be a whole multiple of 

`pagesize`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
    "sourceTypes": [
        {
            "name": "grnry-jdbc",
            "version": "latest",
            "description": "grnry-jdbc source",
            "metadataUri": "maven://org.springframework.cloud.stream.app:jdbc-source-kafka:jar:metadata:2.1.2.RELEASE",
            "uri": "docker://registry:5000/grnry/scdf-apps/grnry-jdbc-source:latest",
            "deployerConfig": {
                "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]",
                "kubernetes.limits.cpu": "400m",
                "kubernetes.requests.cpu": "400m",
                "kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }]",
                "kubernetes.limits.memory": "512Mi",
                "kubernetes.imagePullPolicy": "Always",
                "kubernetes.requests.memory": "512Mi",
                "kubernetes.livenessProbeDelay": "120",
                "kubernetes.readinessProbeDelay": "120"
            },
            "_links": {
                "self": {
                    "href": "https://hostname/harvesters/source-types/grnry-jdbc/latest"
                }
            }
        }
    ],
    "_links": {
        "self": {
            "href": "https://hostname/harvesters/source-types?search=grnry-jdbc&offset=0&pagesize=20&expand="
        }
    }
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Invalid query parameter value." %}
```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'pagesize' must be a natural number but is '-1'.",
    "details": "uri=/harvesters/source-types/grnry-adobe-sftp"
}
```
{% endswagger-response %}

{% swagger-response status="401" description="Token invalid or missing." %}
```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/source-types/grnry-jdbc"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="Missing roles to access this resource." %}
```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/harvesters/source-types/grnry-jdbc"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/harvesters/source-types/:source-type-name/:version" method="get" summary="Get a Specific Version of Source Type" %}
{% swagger-description %}
Get one version of a source type.

\


This request requires the 

`viewer`

 or 

`editor`

 role in at least one project.
{% endswagger-description %}

{% swagger-parameter in="path" name="version" type="string" required="true" %}
Version of the source type.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="source-type-name" type="string" required="true" %}
Name of the source type.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" required="true" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="export" type="string" %}
if set to 

`"true"`

 (not case sensitive), the event type response will only contain properties that a POST body must necessarily contain to create this exact event type. All properties set to default values and all api generated values will be omitted. (Only the api-generated 

`name`

 field will be provided). Default is 

`""`

.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
```
{% endswagger-response %}
{% endswagger %}
