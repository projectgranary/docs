---
description: Denotes release versions of Granary artifacts.
---

# Granary Release Notes

## Release Version Schema

### Granary Platform

* [Semantic Release](https://semver.org/) version on `major.minor` level for Granary platform releases and its corresponding features and documentation.
  * e.g. `1.3` 
* Granary `major.minor` release version defines exactly one combination of Granary components' `patch` level releases.

### Granary Components

* Semantic Release version on `major.minor.patch` level for Docker Container and Helm Chart.
  * e.g. `1.3.2` 
* Component release versions can differ from Granary Platform on `major`, `minor`, and `patch` level.

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
      <td style="text-align:left"></td>
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
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store.md">Segment Table Creator</a>
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