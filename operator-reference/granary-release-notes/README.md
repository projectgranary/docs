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

* [Granary 0.4 Jimi](https://docs.grnry.io/v/0.4-jimi/operator-reference/granary-release-notes)

## 0.5.1 "Amy" - 2019-08-21

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
      <td style="text-align:left"><a href="https://github.com/helm/charts/tree/master/stable/ambassador">Ambassador</a>
      </td>
      <td style="text-align:left">0.40.1</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/snowplow-scala-stream-collector.md">Snowplow Scala Stream Collector API</a>
      </td>
      <td style="text-align:left">0.4.3 (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/spring-cloud-data-flow.md"><b>Spring Cloud Dataflow Server and Skipper</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"><b>0.5.8 (based on Server 2.2.0.RELEASE and Skipper 2.0.3.RELEASE)</b>
        </td>
        <td style="text-align:left">
          <ul>
            <li>Keycloak integration for Server and Skipper&apos;s REST endpoints working,
              fixes known issue GKI_2019_0013.</li>
            <li>Server and Skipper&apos;s REST endpoints configurable to work with TLS
              encryption</li>
          </ul>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Apps</td>
      <td style="text-align:left"><b>0.5.6</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Known issue GKI_2019_0017 fixed</li>
          <li>Jython language working in Scriptable Transform processor</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left">0.4.5</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-feeder.md">Event Feeder</a>
      </td>
      <td style="text-align:left">0.5.2</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/untitled.md">Belt Extractor</a>
      </td>
      <td style="text-align:left">0.5.2</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left"><b>0.5.5</b>
      </td>
      <td style="text-align:left">Search Belt&apos;s via GET /belts?search=&lt;name&gt;, fixes known issue
        GKI_2019_0014</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left">0.4.6</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left">0.5.1</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left">0.4.3</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left">0.4.6 (based on Presto 0.315)</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left"><b>0.4.0</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Profile Explorer working with Profile Types, fixes known issue GKI_2019_0015</li>
          <li>Profile Explorer working with profile link notation as documented, fixes
            known issue GKI_2019_0016</li>
          <li>Belt Editor has three new fields: Event Type, Belt Extractor version,
            python lib requirements</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left"><b>0.4.0</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Profile Explorer working with Profile Types, fixes known issue GKI_2019_0015</li>
          <li>Profile Explorer working with profile link notation as documented, fixes
            known issue GKI_2019_0016</li>
          <li>Belt Editor has three new fields: Event Type, Belt Extractor version,
            python lib requirements</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/citus-postgresql.md">Citus PostgreSQL</a>
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
      <td style="text-align:left"><a href="../installation/kafka-manager.md">Kafka Manager</a>
      </td>
      <td style="text-align:left"><b>0.4.2</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Kafka version 2.x.x supported</li>
          <li>Consumer groups are correctly displayed again</li>
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
      <td style="text-align:left">6.0.2</td>
      <td style="text-align:left">-</td>
    </tr>
  </tbody>
</table>

## 0.5.0 "Amy" - 2019-07-31

| Granary Component | Release Version | Release Notes |
| :--- | :--- | :--- |
| [Ambassador](https://github.com/helm/charts/tree/master/stable/ambassador) | 0.40.1 | - |
| [Snowplow Scala Stream Collector API](../installation/snowplow-scala-stream-collector.md) | 0.4.3 \(based on Snowplow v0.15.0\) | - |
| [Spring Cloud Dataflow](../installation/spring-cloud-data-flow.md) | 0.5.3 | - |
| [Event Store API](../installation/event-store-api.md) | 0.4.5 | - |
| [Event Feeder](../installation/event-feeder.md) | **0.5.2** | Added correct Kafka Header fields for Granary Metadata. |
| [Belt Extractor](../installation/untitled.md) | **0.5.2** | Fixed replay from given offset behavior. |
| [Belt API](../installation/belt-api.md) | **0.5.4** | Removed limitation of belt extractor replication of max. 1 in case offset parameter is set. |
| [Profile Updater](../installation/profile-updater.md) | 0.4.6 | - |
| [Profile Store API](../installation/profile-store-api.md) | 0.5.1 | - |
| [Segment Table Creator](../../developer-reference/dataflow/segment-store/) | 0.4.3 | - |
| [Segment Store API](../installation/segment-store-api.md) | 0.4.6 \(based on Presto 0.315\) | - |
| [GraphQL API](../installation/graphql-api.md) | **0.3.1** | Belt List pagination added. |
| [Granary UI](../installation/granary-ui.md) | **0.3.1** | Belt List pagination added. |
| [Citus PostgreSQL](../installation/citus-postgresql.md) | 8.1.1 | - |
| [Confluent Kafka](https://github.com/confluentinc/cp-helm-charts/tree/master/charts) | 5.1.2 | - |
| [Confluent Zookeeper](https://github.com/confluentinc/cp-helm-charts/tree/master/charts) | 5.1.2 | - |
| [Kafka Manager](../installation/kafka-manager.md) | 0.4.1 | - |
| [Keycloak](https://github.com/helm/charts/tree/master/stable/keycloak) | 4.5.0.Final | - |
| [Grafana](https://github.com/helm/charts/tree/master/stable/grafana) | 6.0.2 | - |

## RC2 0.5 "Amy" - 2019-07-18

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
      <td style="text-align:left"><a href="https://github.com/helm/charts/tree/master/stable/ambassador">Ambassador</a>
      </td>
      <td style="text-align:left">0.40.1</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/snowplow-scala-stream-collector.md">Snowplow Scala Stream Collector API</a>
      </td>
      <td style="text-align:left">0.4.3 (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/spring-cloud-data-flow.md">Spring Cloud Dataflow</a>
      </td>
      <td style="text-align:left"><b>0.5.3</b>
      </td>
      <td style="text-align:left">Dead Letter Queue added, Zipking tracing percentage reduced</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left"><b>0.4.5</b>
      </td>
      <td style="text-align:left">Zipking tracing percentage reduced, docu updated</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-feeder.md">Event Feeder</a>
      </td>
      <td style="text-align:left"><b>0.5.1</b>
      </td>
      <td style="text-align:left">Helm deployment fixed, docu updated</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/untitled.md">Belt Extractor</a>
      </td>
      <td style="text-align:left"><b>0.5.1</b>
      </td>
      <td style="text-align:left">Dead Letter Queue default renamed, docu updated</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left"><b>0.5.3</b>
      </td>
      <td style="text-align:left">Various bugs fixed: state endpoint returns correct values, no zombie belts
        after ugprades</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left"><b>0.4.6</b>
      </td>
      <td style="text-align:left">Various bugs fixed concerning encoding issues.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left"><b>0.5.1</b>
      </td>
      <td style="text-align:left">Zipkin tracing percentage reduced, docu updated</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left">0.4.3</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left"><b>0.4.6 (based on Presto 0.315)</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>increased Presto version to support JSON columns</li>
          <li>Keycloak group-role assignment evaluted correctly</li>
          <li>Number of Keycloak requests reduced</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left"><b>0.3.0</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Belt States fixed</li>
          <li>Export Belt config</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left"><b>0.3.0</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Belt States fixed</li>
          <li>Export Belt config</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/citus-postgresql.md">Citus PostgreSQL</a>
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
      <td style="text-align:left"><a href="../installation/kafka-manager.md">Kafka Manager</a>
      </td>
      <td style="text-align:left">0.4.1</td>
      <td style="text-align:left">-</td>
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
      <td style="text-align:left">6.0.2</td>
      <td style="text-align:left">-</td>
    </tr>
  </tbody>
</table>

## RC1 0.5 "Amy" - 2019-07-03

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
      <td style="text-align:left"><a href="https://github.com/helm/charts/tree/master/stable/ambassador">Ambassador</a>
      </td>
      <td style="text-align:left">0.40.1</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/snowplow-scala-stream-collector.md">Snowplow Scala Stream Collector API</a>
      </td>
      <td style="text-align:left"><b>0.4.3 (based on Snowplow v0.15.0)</b>
      </td>
      <td style="text-align:left">Ingress object updated.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Spring Cloud Dataflow</b>
      </td>
      <td style="text-align:left"><b>0.5.1</b>
      </td>
      <td style="text-align:left">Initial Release of Metadata Processor, Postgresql Eventstore Sink, HTTP
        Source, JDBC Sink, JDBC Source, Log Sink, Scriptable Processor, SFTP Source.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left"><b>0.4.4</b>
      </td>
      <td style="text-align:left">Zipkin Tracing added, ingress object updated.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Event Feeder</b>
      </td>
      <td style="text-align:left"><b>0.5.0</b>
      </td>
      <td style="text-align:left">Initial Release for Event Replay.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/untitled.md">Belt Extractor</a>
      </td>
      <td style="text-align:left"><b>0.5.0</b>
      </td>
      <td style="text-align:left">
        <p></p>
        <ul>
          <li>Request arbitrary profile from within belt function</li>
          <li>Profile Type in Belt Function Runtime</li>
          <li>Read from multiple event types</li>
          <li>Start processing at certain offset</li>
          <li>Metadata in Kafka Header fields</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Belt API</b>
      </td>
      <td style="text-align:left"><b>0.5.0</b>
      </td>
      <td style="text-align:left">Initial Release Belt API.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left"><b>0.4.4</b>
      </td>
      <td style="text-align:left">Ingress object updated.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left"><b>0.5.0</b>
      </td>
      <td style="text-align:left">Zipkin Tracing added, ingress object updated, Profile Type in Profile
        API</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left"><b>0.4.3</b>
      </td>
      <td style="text-align:left">Helm chart added.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left">0.4.3 (based on Presto 0.216)</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left"><b>0.2.0</b>
      </td>
      <td style="text-align:left">Belt UI added</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left"><b>0.2.0</b>
      </td>
      <td style="text-align:left">Belt UI added</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/spring-cloud-data-flow.md">Citus PostgreSQL</a>
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
      <td style="text-align:left"><a href="../installation/kafka-manager.md">Kafka Manager</a>
      </td>
      <td style="text-align:left"><b>0.4.1</b>
      </td>
      <td style="text-align:left">Ingress object updated.</td>
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
      <td style="text-align:left"><b>6.0.2</b>
      </td>
      <td style="text-align:left">Alerting and Prometheus Queries.</td>
    </tr>
  </tbody>
</table>

