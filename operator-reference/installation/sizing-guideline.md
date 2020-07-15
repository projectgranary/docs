---
description: This is a recommendation for the configuration of resources and their limits
---

# Sizing guideline

## Minimal setup

{% hint style="warning" %}
 This is a recommendation to process 500 events per second and 1,000 profile updates per second. Depending on workload requirements may differ.
{% endhint %}

In Kubernetes, the sum of all _requires_ cannot exceed the amount of physically present hardware whereas the _limits_ can be used to over-provision the Kubernetes worker nodes to make better use of idle resources.

Units RAM:

* 1 Gi = 1024Mb
* 1 Mi = 1025Kb

Units CPU:

* 1000m = 1

| Component | Replica | Require RAM | Limit RAM | Require CPU | Limit CPU |
| :--- | :--- | :--- | :--- | :--- | :--- |
| keycloak | 1 | 4Gi | 4Gi | 750m | 750m |
| snowplow | 3 | 1Gi | 1Gi | 300m | 300m |
| spring-cloud-data-flow-server | 1 | 640Mi | 4Gi | 500m | 750m |
| spring-cloud-data-flow-skipper | 1 | 640Mi | 2Gi | 500m | 750m |
| spring-cloud-data-flow-component | 3-n | 256Mi | 256Mi | 50m | 300m |
| harvester-api | 1 | 512Mi | 1Gi | 250m | 500m |
| belt-api | 1 | 512Mi | 1Gi | 250m | 500m |
| belt-extractor | 1-n | 128Mi | 256Mi | 100m | 300m |
| eventstore-api | 1 | 1Gi | 1Gi | 250m | 500m |
| kafka-profile-update | 4 | 1Gi | 1Gi | 100m | 100m |
| profilestore-api | 1 | 1Gi | 1Gi | 250m | 500m |
| reaper | 1 | 512Mi | 1Gi | 100m | 250m |
| segment-management-api | 1 | 512Mi | 1Gi | 100m | 250m |
| segment-manager | 1 | 128Mi | 128Mi | 100m | 100m |
| segment-table-creation | 1-n | 128Mi | 128Mi | 50m | 50m |
| segmentstore-api-coordinator | 1 | 1Gi | 2Gi | 250m | 500m |
| segmentstore-api-worker | 2 | 1Gi | 2Gi | 250m | 500m |
| citus-postgres-manager | 1 | 100Mi | 1Gi | 50m | 200m |
| citus-postgres-master | 1 | 256Mi | 8Gi | 500m | 1 |
| citus-postgres-worker | 2-5 | 256Mi | 4Gi | 250m | 500m |
| kafka-manager | 1 | 256Mi | 1Gi | 100m | 200m |
| kafka-broker | 3 | 4Gi | 4Gi | 250m1 |  |
| zookeper | 3 | 256Mi | 1G | 100m | 1 |
| grnry-ui | 1 | 256Mi | 512Mi | 100m | 250m |
| graphql-api | 1 | 256Mi | 512Mi | 100m | 250m |
| zipkin | 1 | 1Gi | 1Gi | 1 | 1 |
| TOTAL |  | ~ 28Gi |  | ~ 12 |  |
| **TOTAL with safety guard** |  | ~ 32Gi |  | ~ 16 |  |

{% hint style="info" %}
Kubernetes cluster hardware consumption can be reduced and processing speed can be increased in case of managed PostgreSQL offerings are used.
{% endhint %}

{% hint style="info" %}
To ensure a guaranteed Quality of Service,  kubernetes`requests`and `limits`have to be set to the same values. Furthermore, if only specifying `requests`or `limits`, kubernetes will automatically assign matching values for`limits` or `requests` respectively.
{% endhint %}

