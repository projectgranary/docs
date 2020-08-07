---
description: Guide to migrate Granary 0.7 Segment Jobs to Granary 0.8
---

# Segment Job Migration

With previous Granary versions segments where created by starting Cronjobs via Kubernetes, like so:

```text
cronjob:
  schedule: "*/1 * * * *"
  concurrencyPolicy: Forbid
  successfulJobsHistoryLimit: 1
  failedJobsHistoryLimit: 1

image:
  repository: hub.syncier.cloud/grnry/segment-table-creation
  pullSecrets: grnry-base-private-registry-token
  restartPolicy: OnFailure

env:
  SOURCE_SCHEMA_NAME: "public" 
  TARGET_SCHEMA_NAME: "segments" 
  SOURCE_TABLE_NAME: "profilestore" 
  TARGET_SEGMENT_NAME: "demo_segment" 

  COLUMN_PLACEHOLDER: "?" 
  TYPE_SEPARATOR: "::" 
  DEFINITION_SEPARATOR: "=" 
  TRANSFORMATION_SEPARATOR: ";" 
  CITUS_DIST_COL: "correlation_id" 
  TYPE: "pivot" 
  PIVOT_PATHS: "/pathA,/pathB,/pathC"

  PIVOT_TRANSFORMATIONS: "/pathC=substring(?#>>'{}', 4, 4)::text"
  PIVOT_ALLOW_EMPTY: "False" 

  TARGET_SEGMENT_INDEX_SEPARATOR: "|"
  TARGET_SEGMENT_INDEXES: "idx1=(\"/pathA\")"

  DB_HOST: "grnry-citus" 
  DB_PORT:  "5432"  
  DB_USE_SSL: "True"
  PROMETHEUS_PUSHGATEWAY: ""
  PROMETHEUS_JOB: "profilestore_int_test_seg1" 
  DEBUG: "True" 

envFromSecrets:
  DB_USER:  
    secret: grnry-secret
    key: superuser-username
  DB_NAME:  
    secret: grnry-secret
    key: superuser-database
  DB_PASSWORD:  
    secret: grnry-secret
    key: superuser-password
```

In Granary 0.8 the [Segment Management API](../../developer-reference/api-reference/segment-management-api.md) and the [Segment Manager](../../developer-reference/dataflow/segment-store/segment-manager.md) are introduced to reduce the error-proneness and ease the effort of creating and managing segment jobs.

The example above transformed into a Granary 0.8 Segment Job JSON representation:

```text
{
    "metadata": {
        "displayName": "demo segment",
        "description": "This is a demo segment "
    },
    "data": {
        "cronjob": {
            "schedule": "*/1 * * * *"
        },
        "env": {
            "SOURCE_TABLE_NAME": "profilestore",
            "TARGET_SEGMENT_NAME": "demo_segment",
            "SOURCE_WHERE_CLAUSE": "profile_type='opensky' and pit='_latest'",
            "CITUS_DIST_COL": "correlation_id",
            "TYPE": "pivot",
            "PIVOT_PATHS": "/pathA,/pathB,/pathC",
            "PIVOT_TRANSFORMATIONS": "/pathC=substring(?#>>'{}', 4, 4)::text",
            "PIVOT_ALLOW_EMPTY": "False",
            "PROMETHEUS_PUSHGATEWAY": "",
            "PROMETHEUS_JOB": "profilestore_int_test_seg1",
            "DEBUG": "True"
        }
    }
}
```

This JSON snippet needs to be sent to the Segment Management API as HTTP request body.

Many of the properties set before are now default values from [Segment Manager](../../developer-reference/dataflow/segment-store/segment-manager.md).

