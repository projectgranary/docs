---
description: This page describes a run-through of creating a Segment with built-in GRNRY
---

# Segment Creation

To deploy the jobs on the cluster for segment creation we use helm charts. A detailed explanation on how the segment store works can be seen [here](../../developer-reference/dataflow/segment-store/). We will be doing a walk through of the steps to create a segment on Profile store. A sample helm chart for segment creation can be seen [here](https://gitlab.alvary.io/grnry/segment-table-creation/blob/master/helm/values.yaml).

The first step is to create a yaml file which specifies the functionalities to be performed by the segment. A sample yaml file is attached here to demonstrate the capability of the segment.

```text
cronjob:
  schedule: "*/1 * * * *"
  concurrencyPolicy: Forbid
  successfulJobsHistoryLimit: 1
  failedJobsHistoryLimit: 1

image:
  repository: gitlab.alvary.io:5000/grnry/segment-table-creation
  tag: "{{ .Chart.AppVersion }}"
  pullPolicy: IfNotPresent
  pullSecrets: grnry-base-private-registry-token
  restartPolicy: OnFailure

env:
  ############### General Configuration
  SOURCE_SCHEMA_NAME: "public" # schema of source table
  TARGET_SCHEMA_NAME: "segments" # schema for target segment

  SOURCE_TABLE_NAME: "profilestore" # name of source table
  TARGET_SEGMENT_NAME: "profilestore_opensky_segment" # name for target segment, will be overriden if existing

  COLUMN_PLACEHOLDER: "?" # placeholder for the path name to be used in the pivot transformation function
  TYPE_SEPARATOR: "::" # separator between pivot transformation function and result type
  DEFINITION_SEPARATOR: "=" # separator between path name and pivot transformation function
  TRANSFORMATION_SEPARATOR: "|" # separator between path name and pivot transformation function

  CITUS_DIST_COL: "correlation_id" # mandatory: name of the column the source table is and the target segment will be distributed by. Currently, this has to be a single column.

  SOURCE_WHERE_CLAUSE: "profile_type='Open-sky' and pit='_latest'"

  ############### Type Configuration
  TYPE: "pivot" # type of generator, "pivot" or "generic"

  ############### PIVOT Configuration
  # list of paths to be turned into columns; comma-separated
  PIVOT_PATHS: ""
  # list of transformations to apply on pivot paths
  # must be in the format <path><DEFINITION_SEPARATOR><sql expression with COLUMN_PLACEHOLDER><TYPE_SEPARATOR><result type><TRANSFORMATION_SEPARATOR>...
  PIVOT_TRANSFORMATIONS: ""
  PIVOT_ALLOW_EMPTY: "True" # False removes profile entries where all grains do not exist, and thus turn into null
  PIVOT_SEGMENT_WHERE_CLAUSE: "" # condition to further filter the resulting segment. for example, a::text = '\"foo\"'

  ############### GENERIC Configuration
  # columns to be used without transformation
  GENERIC_COLUMNS: "event_id"
  # columns to be created from transformation
  GENERIC_TRANSFORMATIONS: "body_txt=message->'body'::text|body_json=replace(message->'body'#>>'{}', '\', '')::jsonb|headers=message->'headers'::jsonb"

  ############### Index Definition
  TARGET_SEGMENT_INDEX_SEPARATOR: "|"
  # must be in the format <index_name>=<index expression><TARGET_SEGMENT_INDEX_SEPARATOR...
  TARGET_SEGMENT_INDEXES: ""

  ############### Database settings
  DB_HOST: "grnry-pg-citus-master" # citus master host
  DB_PORT:  "5432"  # citus master port
  DB_USE_SSL: "True"

  ############### Logging settings

  PROMETHEUS_PUSHGATEWAY: "" # address of gateway to push prometheus metrics, format '<host>:<port>'
  PROMETHEUS_JOB: "segmentcreation" # job name for pushgateway. the metrics will be grouped by this name.
  DEBUG: "True" # whether to print all database statement and responses

```

After the yaml file has been made, there need to be a few settings on the kubernetes based cluster, which can be viewed [here](../../operator-reference/installation/kubernetes-client-environment-set-up.md). The next step is to deploy the yaml file through helm on the cluster, which is explained [here](../../developer-reference/dataflow/segment-store/segment-table-creation.md). After performing the above mentioned steps, a pod will be deployed on the cluster with the frequency specified in the yaml file.

