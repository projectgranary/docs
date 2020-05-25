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

* [Granary 0.7 Kurt](https://docs.grnry.io/v/0.7-kurt/operator-reference/granary-release-notes)
* [Granary 0.6 Freddie](https://docs.grnry.io/v/0.6-freddie/operator-reference/granary-release-notes)
* [Granary 0.5 Amy](https://docs.grnry.io/v/0.5-amy/operator-reference/granary-release-notes)
* [Granary 0.4 Jimi](https://docs.grnry.io/v/0.4-jimi/operator-reference/granary-release-notes)

## Granary 0.8.0 "Lemmy" - 2020-04-30

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
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">see <a href="https://github.com/datawire/ambassador/releases/tag/v1.0.0">Ambassador Release Notes</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/snowplow-scala-stream-collector.md">Snowplow Scala Stream Collector API</a>
      </td>
      <td style="text-align:left">0.5.1 (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/harvester-api.md"><b>Harvester API</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"><b>0.8.1</b>
        </td>
        <td style="text-align:left">
          <p>Features:</p>
          <ul>
            <li>Management of Event Types</li>
            <li>Management of Harvester Instances</li>
            <li>Provide Metadata of Source Types (no full management yet)</li>
            <li>Extensive default configuration options</li>
            <li>Access to API is secured via Granary Keycloak</li>
          </ul>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/spring-cloud-data-flow.md">Spring Cloud Dataflow Server and Skipper</a>
      </td>
      <td style="text-align:left">0.5.10 (based on Server 2.2.3.RELEASE and Skipper 2.1.4.RELEASE)</td>
      <td
      style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../learning-grnry-1/data-in/how-to-run-a-harvester/getting-started.md">Spring Cloud Data Flow Apps</a>
      </td>
      <td style="text-align:left"><b>0.8.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added a Sink Type that exports events to Postgres and supports batched
            write transaction to database which results in up to 8x faster performance</li>
          <li>Added Topic Source Type to import events from a Kafka topic</li>
          <li>Event Type versionining supported by all Sink Types and Sessionizing Processor</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left"><b>0.7.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added Event Type version to return payload</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-feeder.md">Event Feeder</a>
      </td>
      <td style="text-align:left"><b>0.6.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added Event Type version support</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Restored AWS Aurora compatibility</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/untitled.md">Belt Extractor</a>
      </td>
      <td style="text-align:left"><b>0.8.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Extended <code>_delete</code> operation with Point-in-time configuration
            possibility</li>
          <li>Added an abortion mechanism for long running callback functions</li>
          <li>Added <code>/belts/{id}</code> as default Profile Update <code>origin</code>
          </li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Changed <code>INFO</code> logs in profileclient to <code>DEBUG</code> to reduce
            noise</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left"><b>0.7.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added checks for existing Event Type to <code>POST /belts</code> and <code>PUT /belts/{id}</code>
          </li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Belt updates behave like a <code>PUT</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left"><b>0.5.6</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Extended <code>_delete</code> operation with Point-in-time configuration
            possibility</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Kafka Profile Updater consumers are considered as dead by broker if the
            are too slow in event processing (GKI_2020_10)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left">0.7.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/segment-creation-api.md"><b>Segment Management API</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"><b>0.8.2</b>
        </td>
        <td style="text-align:left">
          <p>Features:</p>
          <ul>
            <li>Introduced API to manage segment custom resources</li>
            <li>Access to API is secured via Granary Keycloak</li>
          </ul>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/segment-manager.md"><b>Segment Manager</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"><b>0.8.4</b>
        </td>
        <td style="text-align:left">
          <p>Features:</p>
          <ul>
            <li>Introduced a Go-languague Kubernetes operator that manages the segment
              custom resources</li>
          </ul>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left">0.7.2</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left">0.4.8 (based on Presto 0.315)</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left">0.7.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left">0.7.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/reaper.md">Reaper</a>
      </td>
      <td style="text-align:left"><b>0.7.3</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added support for Event Type creation during Reaper execution</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Restored AWS Aurora compatibility</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-explorer-ui.md">Event Explorer UI</a>
      </td>
      <td style="text-align:left">0.2.0</td>
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
      <td style="text-align:left"><b>0.4.4</b>
      </td>
      <td style="text-align:left">Updated Helm Chart to be deployable with Helm3 and on Kubernetes &gt;=1.14.</td>
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
      <td style="text-align:left"><a href="../installation/zipkin.md">Zipkin Server</a>
      </td>
      <td style="text-align:left"><b>0.6.2 (based on Open Zipkin 2.12)</b>
      </td>
      <td style="text-align:left">Updated Helm Chart to be deployable with Helm3 and on Kubernetes &gt;=1.14.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/api-reference/lineage-report.md"><b>Data Lineage Report</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"><b>0.8.1</b>
        </td>
        <td style="text-align:left">
          <p>Features:</p>
          <ul>
            <li>Added a Data Lineage Tool that traverses Granary&apos;s APIs to provide
              lineage information on an Event/Grain&apos;s origin</li>
          </ul>
        </td>
    </tr>
  </tbody>
</table>