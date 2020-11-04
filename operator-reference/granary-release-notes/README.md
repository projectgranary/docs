---
description: Denotes release versions of Granary artifacts.
---

# Granary Release Notes

## Release Version Schema

### Granary Platform

* [Semantic Release](https://semver.org/) version on `major.minor` level for Granary platform releases and its corresponding features and documentation.
  * e.g. `1.3` 
* Granary `major.minor.patch` release version defines exactly one combination of Granary components' `patch` level releases.

### Granary Components

* Semantic Release version on `major.minor.patch` level for Docker Container and Helm Chart.
  * e.g. `1.3.2` 
* Component release versions can differ from Granary Platform on `major`, `minor`, and `patch` level.

## Previous Granary Versions

* [Granary 0.8 Lemmy](https://docs.grnry.io/v/0.8-lemmy/operator-reference/granary-release-notes)
* [Granary 0.7 Kurt](https://docs.grnry.io/v/0.7-kurt/operator-reference/granary-release-notes)
* [Granary 0.6 Freddie](https://docs.grnry.io/v/0.6-freddie/operator-reference/granary-release-notes)
* [Granary 0.5 Amy](https://docs.grnry.io/v/0.5-amy/operator-reference/granary-release-notes)
* [Granary 0.4 Jimi](https://docs.grnry.io/v/0.4-jimi/operator-reference/granary-release-notes)

## Granary 0.9.1 "Marie" - 2020-11-06



## Granary 0.9.0 "Marie" - 2020-08-13

In this new release we focused on **shortening the pipeline** **development cycle** and **improving** **platform stability**.

With the new developer experience features, Granary users are now able to

1. **develop and deploy a Harvester data-in pipeline in** **a matter of minutes** using the new **Harvester UI**.
2. **view & debug the data in their pipelines during development** using the new **Topic Browser**.
3. use **a common log format** in Harvesters and Belts in order to **easier find their logs** in log viewer such as Kibana.

The new platform stability features add self-healing capabilities and help Granary operation engineers to

1. run **Kafka Profile Updater** without interruptions and connection losses while **increasing its parallelism at the same resource consumption level** as before.
2. maintain **the database and Kafka connections of all components at all time** by leveraging Kubernetes Probes.
3. deploy and update Granary in a streamlined GitOps fashion via ArgoCD with images and charts from hub.syncier.cloud, Syncier’s new Harbor service.



<table>
  <thead>
    <tr>
      <th style="text-align:left">Granary Component</th>
      <th style="text-align:left">Release Version</th>
      <th style="text-align:left">Release Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><a href="https://github.com/datawire/ambassador-chart">Ambassador</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">see <a href="https://github.com/datawire/ambassador/releases/tag/v1.0.0">Ambassador Release Notes</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/snowplow-scala-stream-collector.md">Snowplow Scala Stream Collector API</a>
      </td>
      <td style="text-align:left"><b>0.6.1</b> (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Probes</li>
          <li>Added a probe that determines availability of Kafka brokers</li>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
          <li>Configured Granary&apos;s common log format</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/harvester-api/">Harvester API</a>
      </td>
      <td style="text-align:left"><b>0.9.4</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Probes</li>
          <li>Added a probe that determines availability of database</li>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
          <li>Configured Granary&apos;s common log format</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>RUNNING_BUT_OUTDATED state returned in case <em>latest</em> event type version
            is used and <em>latest</em> was updated</li>
          <li>Harmonized API return codes with other APIs (GKI_2020_0007, GKI_2020_0008)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/spring-cloud-data-flow.md">Spring Cloud Dataflow Server and Skipper</a>
      </td>
      <td style="text-align:left"><b>0.6.2</b> (based on Server 2.2.3.RELEASE and Skipper 2.1.4.RELEASE)</td>
      <td
      style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Probes</li>
          <li>Added a probe that determines availability of database</li>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
          <li>Configured Granary&apos;s common log format</li>
        </ul>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/harvester-api/getting-started.md">Spring Cloud Data Flow Apps</a>
      </td>
      <td style="text-align:left"><b>0.9.2</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Introduced OpenJ9 JVM to save up to 50% memory consumption</li>
          <li>Added configuration for Kubernetes Probes</li>
          <li>Added a probe that determines availability of Kafka brokers and database
            (GKI_2019_0029)</li>
          <li>Configured Granary&apos;s common log format</li>
        </ul>
        <p></p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left"><b>0.8.2</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Probes</li>
          <li>Added a probe that determines availability of database</li>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
          <li>Configured Granary&apos;s common log format</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Harmonized API return codes with other APIs</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/event-feeder.md">Event Feeder</a>
      </td>
      <td style="text-align:left"><b>0.6.5</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
          <li>Configured Granary&apos;s common log format</li>
        </ul>
        <p></p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/belt-extractor.md">Belt Extractor</a>
      </td>
      <td style="text-align:left"><b>0.9.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Configured Granary&apos;s common log format</li>
          <li>Configured non-root user to run the belt</li>
          <li>Added belt-id as Kafka header of output messages</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left"><b>0.8.2</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Probes for Belt-API and Belts</li>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget for Belt-API and Belts</li>
          <li>Configured Granary&apos;s common log format</li>
          <li>Added logic that Belts are deployed with a non-root user</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Harmonized API return codes with other APIs (GKI_2020_0009)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left"><b>0.6.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Refactored code-base to become a Spring-Boot application</li>
          <li>Added configuration for Kubernetes Probes</li>
          <li>Added a probe that determines availability of Kafka Broker and database</li>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
          <li>Configured Granary&apos;s common log format</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Kafka Profile Updater consumers are considered as dead by broker if the
            are too slow in event processing (GKI_2020_0010)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left"><b>0.8.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Probes</li>
          <li>Added a probe that determines availability of database</li>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
          <li>Configured Granary&apos;s common log format</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Harmonized API return codes with other APIs</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/with-helm/segment-creation-api.md">Segment Management API</a>
      </td>
      <td style="text-align:left"><b>0.9.2</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Probes</li>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
          <li>Configured Granary&apos;s common log format</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Harmonized API return codes with other APIs</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/segment-manager.md">Segment Manager</a>
      </td>
      <td style="text-align:left"><b>0.8.6</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
          <li>Configured Granary&apos;s common log format</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left"><b>0.7.4</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Removed restriction of maximum 63 characters in pivot segment by introducing
            aliases</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left"><b>0.5.0 (based on Presto 0.339)</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Upgraded Presto to version 339</li>
          <li>Enabled Topic browsing (via Kafka connector)</li>
          <li>Added headers to Kafka connector</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left"><b>0.9.2</b>
      </td>
      <td style="text-align:left">See full <a href="https://github.com/syncier/grnry-graphql-api/releases/tag/0.9.0">release notes</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left"><b>0.9.2</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added Harvester creation wizard</li>
          <li>Added Harvester detail &amp; edit view</li>
          <li>Added Harvester state management</li>
          <li>Added restart mechanism for Belts</li>
        </ul>
        <p>See full <a href="https://github.com/syncier/grnry-react-frontend-admin/releases/tag/0.9.0">release notes</a>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/reaper.md">Reaper</a>
      </td>
      <td style="text-align:left"><b>0.7.8</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
          <li>Configured Granary&apos;s common log format</li>
          <li>Added configuration for log level</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/event-explorer-ui.md">Event Explorer UI</a>
      </td>
      <td style="text-align:left">0.2.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/citus-postgresql.md">Citus PostgreSQL</a>
      </td>
      <td style="text-align:left">8.1.1</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/confluentinc/cp-helm-charts/tree/master/charts">Confluent Kafka</a>
      </td>
      <td style="text-align:left">5.1.2</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/confluentinc/cp-helm-charts/tree/master/charts">Confluent Zookeeper</a>
      </td>
      <td style="text-align:left">5.1.2</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/kafka-manager.md">Kafka Manager</a>
      </td>
      <td style="text-align:left"><b>0.5.3</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/helm/charts/tree/master/stable/keycloak">Keycloak</a>
      </td>
      <td style="text-align:left">4.5.0.Final</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/helm/charts/tree/master/stable/grafana">Grafana</a>
      </td>
      <td style="text-align:left"><b>6.6.0</b>
      </td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/zipkin.md">Zipkin Server</a>
      </td>
      <td style="text-align:left"><b>0.6.3</b> (based on Open Zipkin 2.12)</td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added configuration for Kubernetes Tolerations, Affinity, NodeSelector,
            Security Context and podDisruptionBudget</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/api-reference/lineage-report.md">Data Lineage Report</a>
      </td>
      <td style="text-align:left">0.8.1</td>
      <td style="text-align:left">-</td>
    </tr>
  </tbody>
</table>

