---
description: Explanation of migration strategies between Granary platform versions.
---

# Migration guide

## Upgrade from Granary 0.8 "Lemmy" to Granary 0.9 "Marie"

### Update Granary Components

Update all remaining Granary components to their Granary 0.9 version as denoted in the [release notes](../granary-release-notes/). Bold versions indicate an update.

#### Harvesters

To update the SCDF apps, you need to follow these steps first:

1. [Register ](../installation/with-helm/spring-cloud-data-flow.md#registering-the-grnry-scdf-apps)new version of [mandatory SCDF apps](../installation/with-helm/spring-cloud-data-flow.md#list-of-all-mandatory-scdf-apps) with Spring Cloud Data Flow server
2. [Import SQL script for Source Types](../installation/with-helm/harvester-api/source-types.md#registering-a-source-type-with-a-new-version-of-an-existing-source-app) to Source Type database, latest SQL see here
3. Update defaults for SCDF app versions in Harvester API Helm deployment
4. Restart Harvester API, if no errors occur, update has been successful
   1. If errors occur, revisit steps 1-3 and check for issues

Once this is done, you can update running Harvester instance models via [PUT API call](../../developer-reference/api-reference/harvester-api.md#update-harvester-instance). Restart the Harvester afterwards.

Advocate the usage of the [new common log format for the transform scripts](../../learning-grnry-1/data-in/best-practices-1/logging.md) among use case developers. 

#### Belts

To update the Belts, you need to follow these steps first:

1. Update default for Belt version Belt API Helm deployment
2. Restart Belt API

Once this is done, you can update running Belt models via [PUT API call](../../developer-reference/api-reference/belt-api.md#updates-a-belt-by-id). Restart the Belt afterwards.

Advocate the usage of the [new common log format for the callback scripts](../../learning-grnry-1/using-data-in-granary/best-practices/logging.md) among use case developers.

#### Segments

Update running Segment definitions via [PUT API call](../../developer-reference/api-reference/segment-management-api.md#update-a-segment-job). Wait for the next scheduled execution to get the changes applied.

### Update Segment Store API \(Presto\)

To enable the Kafka topic browsing feature of Granary 0.9, you need to add this configuration to your Helm deployment:

```text
kafka:
  enabled: true
  tables: snowplow,topic1,topic2
```

| Key | Default value | Description |
| :--- | :--- | :--- |
| `kafka.enabled` | true | Switch to enable/disable Kafka connector. |
| `kafka.tables` | snowplow,topic1,topic2 | List of topic names that are queryable via Presto. If left empty, all Kafka topics can be viewed given the user has a Keycloak role to view the respective topic. |

### Update Kafka Profile Updater

Through the refactoring of Profile Updater, there are some new fields in its Helm Chart:

```text
kafkaProfileUpdate:
  concurrency: 1
  sendAllExceptionsToDLQ: false
  logLongRunningOperationsMs: 1000
```

| Key | Default value | Description |
| :--- | :--- | :--- |
| `kafkaProfileUpdate.concurrency` | 1 | Denotes the number of Kafka consumers per Profile Updater pod. |
| `kafkaProfileUpdate.sendAllExceptionsToDLQ` | false | Determines if all exceptions in profile updater's database interaction are directly written to the Dead Letter Queue or  if erroneous updates are being retried. |
| `kafkaProfileUpdate.loglongRunningOperstionsMs` | 1000 | Threshold to log long running database operations. |

See[ Profile Updater documentation](../../developer-reference/dataflow/profile-store/#when-is-something-written-to-the-dead-letter-queue) for more information. Override the defaults in your Helm deployment in case a different behavior is desired.

There is a breaking change in the Helm deployment imagePullSecrets expects a `List` and no longer a `String`:

```text
imagePullSecrets: 
- grnry-dockerconfig
```

### Update Kafka Manager

There is a breaking change in the Helm deployment imagePullSecrets expects a `List` and no longer a `String`:

```text
imagePullSecrets: 
- grnry-dockerconfig
```

### **Kubernetes Probe**

Finally pay attention to the newly added [Kubernetes Probes](../site-reliability/kubernetes-liveness-readiness-probes.md) which might trigger unexpected behavior at first.

**That's it.**

