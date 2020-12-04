---
description: REST API to manage segment jobs via k8s custom resources.
---

# Segment Management API

## Paths

* GET /segments
* GET /segments/{id}
* POST /segments
* POST /segments/{id}
* PUT /segments/{id}
* DELETE /segments/{id}

{% hint style="info" %}
Feel free to check out the [openapi schema](https://github.com/syncier/grnry-segment-management-api/blob/master/definitions/openapi.yaml).
{% endhint %}

{% hint style="warning" %}
Important: Incompatible segment custom resources are omitted from all responses and not updatable via the API. This can happen e.g. when the custom resources does not have annotations.
{% endhint %}

## Segment Job Sample

This section provides you with a sample object for the `data` attribute. All available attributes are defined in the [Segment Manager](../dataflow/segment-store/segment-manager.md) .

**Example**:

```text
{

    "cronjob": {
        "schedule": "*/1 * * * *"
    },
    "env": {
        "SOURCE_TABLE_NAME": "eventstore",
        "TARGET_SEGMENT_NAME": "eventstore_demo_seg",
        "SOURCE_WHERE_CLAUSE": "event_harvester = 'adobe'",
        "CITUS_DIST_COL": "correlation_id",
        "TYPE": "generic",
        "GENERIC_COLUMNS": "event_id,event_harvester",
        "GENERIC_TRANSFORMATIONS": "theval01=message->'val01'::jsonb|theval02=upper(message->'val02'#>>'{}')::text",
        "TARGET_SEGMENT_INDEXES": "idx1=(event_id, theval02)",
        "PROMETHEUS_PUSHGATEWAY": "prometheus-pushgateway.monitoring.svc.cluster.local:9091",
        "PROMETHEUS_JOB": "eventstore_int_test_seg1",
        "DEBUG": "True"
    }

    "envFromSecretes": {
        "DBUser": {
            "key": "user",
            "secret": "db-secret"
        }
    }
}
```

## API Methods

Consult the [Granary Access Clients Reference](../../operator-reference/identity-and-access-management/granary-access-clients.md#segment-management-api) for default roles a user needs to interact with the Segment Management API. However, [different roles](../../operator-reference/identity-and-access-management/granary-access-clients.md#jdbc-api-a-k-a-segment-store-api) apply and are required to read the created Segments via Segment Store API.

{% api-method method="get" host="https://api.grnry.io" path="/segments" %}
{% api-method-summary %}
Get Segment jobs
{% endapi-method-summary %}

{% api-method-description %}
Get all segment jobs sorted by id.  
This request requires you to have either the correct `viewer` or `editor` role.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token required.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="pagesize" type="string" required=false %}
Number of segment jobs to be returned. Default `20`
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="string" required=false %}
A cursor to define the starting point of your request. Default `0`
{% endapi-method-parameter %}

{% api-method-parameter name="search" type="string" %}
Fuzzy Search on the `displayName` to filter the list of returned segment jobs.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "items": [{
        "id": "demo-segment-95he0",
        "metadata": {
            "displayName": "Demo Segment",
            "description": "This is a demo segment ",
            "createdAt": "2020-03-18T18:08:31.850",
            "modifiedAt": "2020-03-18T18:09:17.179",
            "createdBy": {
                "name": "foo",
                "email": "for@bar.io"
            },
            "modifiedBy": {
                "name": "foo",
                "email": "foo@bar.io"
            }
        },
        "roles": {
            "editor": [
                "segment_job_edit",
                "demo_role"
            ],
            "viewer": [
                "segment_job_view"
            ]
        },
        "data": {
            "cronjob": {
                "schedule": "*/10 * * * *"
            },
            "env": {
                "CITUS_DIST_COL": "correlation_id",
                "DEBUG": "True",
                "GENERIC_COLUMNS": "event_id,event_harvester",
                "GENERIC_TRANSFORMATIONS": "theval01=message->'val01'::jsonb|theval02=upper(message->'val02'#>>'{}')::text",
                "PROMETHEUS_JOB": "eventstore_int_test_seg1",
                "PROMETHEUS_PUSHGATEWAY": "prometheus-pushgateway.monitoring.svc.cluster.local:9091",
                "SOURCE_TABLE_NAME": "eventstore",
                "SOURCE_WHERE_CLAUSE": "event_harvester = 'adobe'",
                "TARGET_SEGMENT_INDEXES": "idx1=(event_id, theval02)",
                "TARGET_SEGMENT_NAME": "eventstore_demo_seg",
                "TYPE": "generic"
            }
        },
        "links": {
            "self": "http://hostname:8080/segments/demo-segment-95he0?action=self"
        },
        "labels": [
            "customer",
            "demo"
        ]
    }],
    "links": {
        "self": "http://hostname:8080/segments?offset=0&pagesize=1&search=demo&action=self"
    },
    "totalCount": 1
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid query parameter value.
{% endapi-method-response-example-description %}

```
{
    "timestamp": "2020-12-02T20:14:45+01:00",
    "message": "Parameter 'pagesize' 'null' could not be parsed to int.",
    "type": "bad_parameter_value",
    "traceId": null,
    "invalidParams": [
        {
            "name": "pagesize",
            "reason": "'null' could not be parsed to int"
        }
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token missing.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Token invalid.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "timestamp":1586949269381,
    "type": "unexpected_error",
    "message":"An unexpected error occured."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/segments/{id}" %}
{% api-method-summary %}
Get Segment Job by ID
{% endapi-method-summary %}

{% api-method-description %}
Get a segment job by its id.  
This request requires you to have either the correct `viewer` or `editor` role.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
Identifier of the segment job.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token required
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="export" type="string" required=false %}
If set to `true` \(not case sensitive\), the segment job response will only contain properties a POST body must necessarily contain to create this exact segment job. All properties set to default values and all segment-manager generated properties will be omitted. \(only the api-generated id will be provided\). Default is `false` .
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Response when export flag is `false`
{% endapi-method-response-example-description %}

```text
{
    "id": "demo-segment-95he0",
    "metadata": {
        "displayName": "Demo Segment",
        "description": "This is a demo segment ",
        "createdAt": "2020-03-18T18:08:31.850",
        "modifiedAt": "2020-03-18T18:09:17.179",
        "createdBy": {
            "name": "foo",
            "email": "for@bar.io"
        },
        "modifiedBy": {
            "name": "foo",
            "email": "foo@bar.io"
        }
    },
    "roles": {
        "editor": [
            "segment_job_edit",
            "demo_role"
        ],
        "viewer": [
            "segment_job_view"
        ]
    },
    "data": {
        "cronjob": {
            "schedule": "*/10 * * * *"
        },
        "env": {
            "CITUS_DIST_COL": "correlation_id",
            "DEBUG": "True",
            "GENERIC_COLUMNS": "event_id,event_harvester",
            "GENERIC_TRANSFORMATIONS": "theval01=message->'val01'::jsonb|theval02=upper(message->'val02'#>>'{}')::text",
            "PROMETHEUS_JOB": "eventstore_int_test_seg1",
            "PROMETHEUS_PUSHGATEWAY": "prometheus-pushgateway.monitoring.svc.cluster.local:9091",
            "SOURCE_TABLE_NAME": "eventstore",
            "SOURCE_WHERE_CLAUSE": "event_harvester = 'adobe'",
            "TARGET_SEGMENT_INDEXES": "idx1=(event_id, theval02)",
            "TARGET_SEGMENT_NAME": "eventstore_demo_seg",
            "TYPE": "generic"
        }
    },
    "links": {
        "self": "http://hostname:8080/segments/demo-segment-95he0?action=self"
    },
    "labels": [
        "customer",
        "demo"
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token missing.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Token invalid or missing roles to access this resource.
{% endapi-method-response-example-description %}

```text
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Segment with given ID does not exist.
{% endapi-method-response-example-description %}

```
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Segment with ID 'demo-segment-95he0' not found."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "timestamp":1586949269381,
    "type": "unexpected_error",
    "message":"An unexpected error occured."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.grnry.io" path="/segments/:id" %}
{% api-method-summary %}
Create Segment Job
{% endapi-method-summary %}

{% api-method-description %}
Create a new segment job
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=false %}
Create a new segment job with the provided `id` . It needs to be a valid `DNS-1123` subdomain and be between `4-134` in length.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token required.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="data" type="object" required=true %}
Data describing your segment job . See below for more information.
{% endapi-method-parameter %}

{% api-method-parameter name="labels" type="array" required=false %}
Array of strings to identify this segment job easier.  
Example: `["customerA", "usecaseB"]`
{% endapi-method-parameter %}

{% api-method-parameter name="roles" type="object" required=false %}
Roles that can edit and view this segment job.`{    
"editor": ["string"],    
"viewer": ["string"]    
}`Cannot be `""` or `[]` .
{% endapi-method-parameter %}

{% api-method-parameter name="metadata" type="object" required=true %}
Metadata describing the segment job.displayName: required  
description: optional`{    
"description": "string",    
"displayName": "string"    
}`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "id": "demo-segment-95he0",
    "metadata": {
        "displayName": "Demo Segment",
        "description": "This is a demo segment ",
        "createdAt": "2020-03-18T18:08:31.850",
        "modifiedAt": "",
        "createdBy": {
            "name": "foo",
            "email": "for@bar.io"
        },
        "modifiedBy": {
            "name": "",
            "email": ""
        }
    },
    "roles": {
        "editor": [
            "segment_job_edit"
        ],
        "viewer": [
            "segment_job_view"
        ]
    },
    "data": {
        "cronjob": {
            "schedule": "*/1 * * * *"
        },
        "env": {
            "CITUS_DIST_COL": "correlation_id",
            "DEBUG": "True",
            "GENERIC_COLUMNS": "event_id,event_harvester",
            "GENERIC_TRANSFORMATIONS": "theval01=message->'val01'::jsonb|theval02=upper(message->'val02'#>>'{}')::text",
            "PROMETHEUS_JOB": "eventstore_int_test_seg1",
            "PROMETHEUS_PUSHGATEWAY": "prometheus-pushgateway.monitoring.svc.cluster.local:9091",
            "SOURCE_TABLE_NAME": "eventstore",
            "SOURCE_WHERE_CLAUSE": "event_harvester = 'adobe'",
            "TARGET_SEGMENT_INDEXES": "idx1=(event_id, theval02)",
            "TARGET_SEGMENT_NAME": "eventstore_demo_seg",
            "TYPE": "generic"
        }
    },
    "links": {
        "self": "http://localhost:8080/segments/demo-segment-95he0?action=self"
    },
    "labels": []
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If required parameters are missing or parameters are invalid.
{% endapi-method-response-example-description %}

```text
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'metadata.displayName' must not be empty."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token missing.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Token invalid.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
If ID is not unique.
{% endapi-method-response-example-description %}

```text
{
    "timestamp": 1587303130850,
    "type": "entity_already_exists"
    "message": "Segment 'Demo Segment' already exists.",
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "timestamp":1586949269381,
    "type": "unexpected_error",
    "message":"An unexpected error occured."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="put" host="https://api.grnry.io" path="/segments/{id}" %}
{% api-method-summary %}
Update a Segment Job
{% endapi-method-summary %}

{% api-method-description %}
Updates a segment job.  
This request requires you to have the correct `editor` role.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
Identifier of the segment job.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token required.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="data" type="object" required=true %}
Data describing your segment job. Overwrites all values, same validation as CREATE. See below for more information.
{% endapi-method-parameter %}

{% api-method-parameter name="labels" type="array" required=false %}
Array of strings to identify this segment job easier.  
Overwrites all labels with the given value. If not present in payload overwrites with `[]`
{% endapi-method-parameter %}

{% api-method-parameter name="roles" type="object" required=false %}
Roles that can edit and view the segment job.  
Overwrites `editor` and `viewer` if added, otherwise falls back to original version.
{% endapi-method-parameter %}

{% api-method-parameter name="metadata" type="object" required=true %}
Metadata describing the segment job.  
Overwrites `displayName` and `description`.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "id": "demo-segment-95he0",
    "metadata": {
        "displayName": "Demo Segment",
        "description": "This is a demo segment ",
        "createdAt": "2020-03-18T18:08:31.850",
        "modifiedAt": "2020-03-18T18:09:17.179",
        "createdBy": {
            "name": "foo",
            "email": "for@bar.io"
        },
        "modifiedBy": {
            "name": "foo",
            "email": "foo@bar.io"
        }
    },
    "roles": {
        "editor": [
            "segment_job_edit",
            "demo_role"
        ],
        "viewer": [
            "segment_job_view"
        ]
    },
    "data": {
        "cronjob": {
            "schedule": "*/10 * * * *"
        },
        "env": {
            "CITUS_DIST_COL": "correlation_id",
            "DEBUG": "True",
            "GENERIC_COLUMNS": "event_id,event_harvester",
            "GENERIC_TRANSFORMATIONS": "theval01=message->'val01'::jsonb|theval02=upper(message->'val02'#>>'{}')::text",
            "PROMETHEUS_JOB": "eventstore_int_test_seg1",
            "PROMETHEUS_PUSHGATEWAY": "prometheus-pushgateway.monitoring.svc.cluster.local:9091",
            "SOURCE_TABLE_NAME": "eventstore",
            "SOURCE_WHERE_CLAUSE": "event_harvester = 'adobe'",
            "TARGET_SEGMENT_INDEXES": "idx1=(event_id, theval02)",
            "TARGET_SEGMENT_NAME": "eventstore_demo_seg",
            "TYPE": "generic"
        }
    },
    "links": {
        "self": "http://localhost:8080/segments/demo-segment-95he0?action=self"
    },
    "labels": [
        "customer",
        "demo"
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
If required parameters are missing or parameters are invalid.
{% endapi-method-response-example-description %}

```text
{
    "timestamp": 1587302499600,
    "type": "bad_parameter_value",
    "message": "Parameter 'metadata.displayName' must not be empty."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Token missing.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Token invalid or missing roles to access this resource.
{% endapi-method-response-example-description %}

```text
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Segment with given ID does not exist.
{% endapi-method-response-example-description %}

```text
{
    "timestamp": 1587302671116,
    "type": "entity_not_found",
    "message": "Segment with ID 'demo-segment-95he0' not found."
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "timestamp":1586949269381,
    "type": "unexpected_error",
    "message":"An unexpected error occured."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host="https://api.grnry.io" path="/segments/{id}" %}
{% api-method-summary %}
Delete a Segment Job
{% endapi-method-summary %}

{% api-method-description %}
Deletes a segment job.  
This request requires you to have the correct `editor` role.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
Identifier of the segment job.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=false %}
Authentication token required.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Token invalid or missing roles to access this resource.
{% endapi-method-response-example-description %}

```text
{
    "timestamp":1586949273019,
    "type": "entity_not_accessible",
    "message": "Access forbidden due to missing roles.",
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "timestamp":1586949269381,
    "type": "unexpected_error",
    "message":"An unexpected error occured."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



