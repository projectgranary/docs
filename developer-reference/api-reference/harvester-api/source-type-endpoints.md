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

{% api-method method="get" host="https://api.grnry.io" path="/harvesters/source-types" %}
{% api-method-summary %}
Get all Source Types
{% endapi-method-summary %}

{% api-method-description %}
Get latest version of all source-types.  
This request requires the role `source_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="search" type="string" required=false %}
Filter source types by names containing this search term Default is `""`.
{% endapi-method-parameter %}

{% api-method-parameter name="pagesize" type="number" required=false %}
Number of source types returned per page. Default is `20`.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="number" required=false %}
Offset of the requested page. Default is `0`. Must be a whole multiple of `pagesize`. 
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
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
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid query parameter value.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'pagesize' must be a natural number but is '-1'.",
    "details": "uri=/harvesters/source-types"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/source-types"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Missing roles to access this resource.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details": "uri=/harvesters/source-types"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/harvesters/source-types/:source-type-name" %}
{% api-method-summary %}
Get all versions of an Source Type
{% endapi-method-summary %}

{% api-method-description %}
Get all versions of a given source type.  
This request requires the role `source_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="source-type-name" type="string" required=true %}
Name of the source type.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="pagesize" type="number" required=false %}
Number of source types returned per page. Default is  
`20` .
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="number" required=false %}
Offset of the requested page. Default is `0` . Must be a whole multiple of `pagesize`.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
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
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid query parameter value.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'pagesize' must be a natural number but is '-1'.",
    "details": "uri=/harvesters/source-types/grnry-adobe-sftp"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token invalid or missing.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1586941626155,
    "type": "authentication_error",
    "message": "Authentication failed.",
    "details": "uri=/harvesters/source-types/grnry-jdbc"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Missing roles to access this resource.
{% endapi-method-response-example-description %}

```
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
    "details":"uri=/harvesters/source-types/grnry-jdbc"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/harvesters/source-types/:source-type-name/:version" %}
{% api-method-summary %}
Get a Specific Version of Source Type
{% endapi-method-summary %}

{% api-method-description %}
Get one version of a source type.  
This request requires the role `source_type_read`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="version" type="string" required=true %}
Version of the source type.
{% endapi-method-parameter %}

{% api-method-parameter name="source-type-name" type="string" required=true %}
Name of the source type.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="export" type="string" required=false %}
if set to `"true"` \(not case sensitive\), the event type response will only contain properties that a POST body must necessarily contain to create this exact event type. All properties set to default values and all api generated values will be omitted. \(Only the api-generated `name` field will be provided\). Default is `""`.
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

