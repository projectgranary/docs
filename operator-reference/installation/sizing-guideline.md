---
description: This is a recommendation for the configuration of resources and their limits
---

# Sizing guideline

## Minimal setup

{% hint style="warning" %}
 This is a recommendation to process > 500 events per second and up to 1,000 profile updates per second. Depending on workload requirements may differ.
{% endhint %}

In Kubernetes, the sum of all _requires _cannot exceed the amount of physically present hardware whereas the _limits _can be used to over-provision the Kubernetes worker nodes to make better use of idle resources.

Units RAM:

* 1 Gi = 1024Mi
* 1 Mi = 1024Ki

Units CPU:

* 1.0 = 1000m
* 0.1 = 100m

Please refer to the [Kubernetes documentation](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes) on full details on resource units.

### Platform components

| Component                      | Replica | Require RAM | Limit RAM | Require CPU | Limit CPU |
| ------------------------------ | ------- | ----------- | --------- | ----------- | --------- |
| keycloak                       | 1       | 1Gi         | 4Gi       | 250m        | 750m      |
| snowplow\*                     | 3       | 512Mi       | 512Mi     | 250m        | 250m      |
| SCDF server                    | 1       | 640Mi       | 4Gi       | 500m        | 1000m     |
| SCDF skipper                   | 1       | 640Mi       | 2Gi       | 500m        | 1000m     |
| harvester-api                  | 1       | 512Mi       | 1Gi       | 100m        | 250m      |
| belt-api                       | 1       | 512Mi       | 1Gi       | 100m        | 250m      |
| eventstore-api                 | 1       | 512Mi       | 1Gi       | 100m        | 250m      |
| kafka-profile-update\*         | 1-n     | 1Gi         | 1Gi       | 250m        | 250m      |
| profilestore-api               | 1       | 512Mi       | 1Gi       | 100m        | 250m      |
| reaper                         | 1       | 512Mi       | 1Gi       | 100m        | 250m      |
| project-management-api         | 1       | 512Mi       | 1Gi       | 100m        | 250m      |
| segmentstore-api-coordinator\* | 1       | 2Gi         | 2Gi       | 250m        | 250m      |
| segmentstore-api-worker\*      | 2       | 2Gi         | 2Gi       | 250m        | 250m      |
| postgres                       | 1       | 8Gi         | 8Gi       | 2000m       | 2000m     |
| kafka-manager                  | 1       | 128Mi       | 256Mi     | 100m        | 100m      |
| kafka-connect                  | 1       | 512Mi       | 1Gi       | 250m        | 500m      |
| kafka-schema-registry          | 1       | 256Mi       | 512Mi     | 100m        | 250m      |
| kafka-broker\*                 | 3       | 4Gi         | 4Gi       | 1000m       | 1000m     |
| zookeper\*                     | 3       | 1Gi         | 1Gi       | 500m        | 500m      |
| grnry-ui                       | 1       | 256Mi       | 512Mi     | 100m        | 250m      |
| graphql-api                    | 1       | 256Mi       | 512Mi     | 100m        | 250m      |
| zipkin                         | 1       | 1Gi         | 8Gi       | 500m        | 1000m     |
| TOTAL                          |         | \~ 38Gi     |           | \~ 13       |           |
| **TOTAL with safety guard**    |         | \~ 45Gi     |           | \~ 15.0     |           |

{% hint style="warning" %}
For stability reasons, we recommend to set _require_ == _limit_ for central platform components (marked with \* above). In Kubernetes, this setting makes those components least likely to be evicted in-case of resource scarceness.
{% endhint %}

{% hint style="info" %}
Kubernetes cluster hardware consumption can be reduced and processing speed can be increased in case of managed PostgreSQL offerings are used.
{% endhint %}

### Data pipeline components

| Component                                                                                | Replica | Require RAM | Limit RAM | Require CPU | Limit CPU |
| ---------------------------------------------------------------------------------------- | ------- | ----------- | --------- | ----------- | --------- |
| <p>SCDF component</p><p><em>(each harvester consists of min. 4 SCDF components)</em></p> | 4-n     | 256Mi       | 512Mi     | 250m        | 500m      |
| python-belt                                                                              | 1-n     | 256Mi       | 256Mi     | 100m        | 250m      |
| dbt-segment-belt                                                                         | 1-n     | 128Mi       | 128Mi     | 50m         | 50m       |
| **TOTAL per data pipeline**                                                              |         | \~ 1.5Gi    |           | \~ 1.25     |           |
