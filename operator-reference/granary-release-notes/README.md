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

## Granary 0.6 "Freddie" - 2019-10-02

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
      <td style="text-align:left"><b>0.5.0</b> (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">Kafka Interceptor for Zipkin Tracing added and Zipkin tracing configurable
        via Helm.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/spring-cloud-data-flow.md">Spring Cloud Dataflow Server and Skipper</a>
      </td>
      <td style="text-align:left">0.5.8 (based on Server 2.2.0.RELEASE and Skipper 2.0.3.RELEASE)</td>
      <td
      style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Apps</td>
      <td style="text-align:left"><b>0.6.0</b>
      </td>
      <td style="text-align:left">Adobe Harvester added.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left"><b>0.5.1</b>
      </td>
      <td style="text-align:left">Zipkin tracing configurable via Helm.</td>
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
      <td style="text-align:left"><b>0.6.2</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>New Profile Update operations added: _set_if_not_exist, _set_with_history_distinct,
            _array_put_with_history_distinct</li>
          <li>Grain Type cannot be altered after initial set.</li>
          <li>Zipkin Tracing added.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left"><b>0.6.2</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Belt Destination Topic configurable in Belt API Deployments. Requires
            database schema update.</li>
          <li>Zipkin tracing configurable via Helm.</li>
          <li>Fixed various bugs in Kubernetes deployment configuration of Belt extractor.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left"><b>0.5.1</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Event Priorization for Batch Profile Update topic added.</li>
          <li>New Profile Update operations added: _set_if_not_exist, _set_with_history_distinct,
            _array_put_with_history_distinct</li>
          <li>Grain Type cannot be altered after initial set.</li>
          <li>Zipkin tracing added.</li>
          <li>Kubernetes Horizontal Pod Autoscaling based on Kafka topic lag added.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left"><b>0.5.3</b>
      </td>
      <td style="text-align:left">Zipkin tracing configurable via Helm.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left"><b>0.5.1</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Pivot Segments on Eventstore added.</li>
          <li>Indices on Segments added.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left"><b>0.4.7</b> (based on Presto 0.315)</td>
      <td style="text-align:left">Prometheus metrics exporter for Segment Store API access added.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left">0.4.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left"><b>0.4.1</b>
      </td>
      <td style="text-align:left">Belt creation bug fixed.</td>
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
      <td style="text-align:left">0.4.2</td>
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
    <tr>
      <td style="text-align:left"><b>Zipkin Server</b>
      </td>
      <td style="text-align:left"><b>0.6.0 (based on Open Zipkin 2.12)</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Zipkin Server and UI for tracing and evaluation added.</li>
          <li>Basic Authentication for Zipkin Server and UI added.</li>
          <li>Storage supports in-memory or Elasticsearch 7.3.0.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

