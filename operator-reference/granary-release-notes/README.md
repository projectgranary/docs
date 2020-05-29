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

* [Granary 0.6 Freddie](https://docs.grnry.io/v/0.6-freddie/operator-reference/granary-release-notes)
* [Granary 0.5 Amy](https://docs.grnry.io/v/0.5-amy/operator-reference/granary-release-notes)
* [Granary 0.4 Jimi](https://docs.grnry.io/v/0.4-jimi/operator-reference/granary-release-notes)

## Granary 0.7.2 "Kurt" - 2020-04-08



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
      <td style="text-align:left"><b>0.5.1</b> (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>incoming message buffer size configurable to receive larger messages</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/spring-cloud-data-flow.md">Spring Cloud Dataflow Server and Skipper</a>
      </td>
      <td style="text-align:left"><b>0.5.10</b> (based on Server 2.2.3.RELEASE and Skipper 2.1.4.RELEASE)</td>
      <td
      style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Updated SCDF server to 2.2.3.RELEASE</li>
          <li>Updated SCDF skipper to 2.1.4.RELEASE</li>
          <li>Maven repository configuration added to SCDF server to make Metadata JARs
            available for SCDF Apps</li>
        </ul>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Apps</td>
      <td style="text-align:left"><b>0.7.1</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>reset offsets bug, see GKI_2020_0001</li>
          <li>native encoding bug in Sessionizing processor</li>
          <li>empty string payload bug, see GKI_2020_0002</li>
          <li>removed verbose logging</li>
        </ul>
        <p>Features:</p>
        <ul>
          <li>added S3 Source Type</li>
          <li>
            <p>added Adobe S3 Source Type</p>
            <p></p>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left"><b>0.6.4</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>total count and next link bug, see GKI_2020_0003</li>
          <li>incomplete JSON payload on GET <code>/events/{correlation-id}/{event-id}</code>,
            see GKI_2020_0004</li>
        </ul>
      </td>
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
      <td style="text-align:left">0.7.1</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left"><b>0.6.6</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>total count and next link bug, see GKI_2020_0003</li>
          <li>Belt updates behave like a <code>PUT</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left"><b>0.5.4</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>ThrottleControl blocks fast lane topic and batch consumption, see GKI_2020_0005</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left"><b>0.7.0</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>total count and next link, see GKI_2020_0003</li>
        </ul>
        <p>Features:</p>
        <ul>
          <li>Fragement query parameter added</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left"><b>0.7.2</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>failing typecasts on columns containing special characters</li>
          <li><code>seperator</code> typo to <code>separator</code> in helm values, readme
            and code</li>
        </ul>
        <p>Features:</p>
        <ul>
          <li>Line breaks supported in generic transformations</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left"><b>0.4.8</b> (based on Presto 0.315)</td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Adds configmap mount point ability to provide JKS file mount containing
            TLS cert to Helm chart</li>
          <li>Adds extra env, container and mounts to Helm chart</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left"><b>0.7.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Added support for nested link Grains (<a href="https://gitlab.alvary.io/grnry/graphql-api/-/commit/4fa0b1a88b91eecca8d22c02926edfee9e3a9faa">4fa0b1a8</a>)</li>
          <li>Added basic support for interacting with <code>eventTypes</code> endpoint
            of HarvesterAPI (<a href="https://gitlab.alvary.io/grnry/graphql-api/-/commit/07c3cfd592744e5176a778126267455728f3cb90">07c3cfd5</a>)</li>
        </ul>
        <p>Other:</p>
        <ul>
          <li>Switched to using TypeScript. Consider this when contributing to the codebase.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left"><b>0.7.0</b>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>Features:</p>
        <ul>
          <li>Belts
            <ul>
              <li>Added <code>EventTypeNameSelector</code> to BeltEdit view (currently feature-flagged
                by <code>HARVESTER_API</code> flag). (<a href="https://gitlab.alvary.io/grnry/react-frontend-admin/-/commit/80c064d1a7cbd89b7a26d8e4596b3deff478f656">80c064d1</a>)</li>
              <li>Added <code>GrafanaLink</code> to list items and detail views. Feature-flagged
                by <code>GRAFANA</code> flag. (<a href="https://gitlab.alvary.io/grnry/react-frontend-admin/-/commit/49c4533586fd4713b7ce7c930c4cd857e57f398e">49c45335</a>)</li>
              <li> <code>BeltStateControl</code> checks against the users credentials to either
                show or hide the change state actions (<a href="https://gitlab.alvary.io/grnry/react-frontend-admin/-/commit/65fdfd4b8515a9d230f3855cde6dc264b6fda18f">65fdfd4b</a>)</li>
            </ul>
          </li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Belts
            <ul>
              <li> <code>CopyBeltConfig</code>always fetches fresh data from the API, so
                no stale JSON will be shown anymore (<a href="https://gitlab.alvary.io/grnry/react-frontend-admin/-/commit/c070f57927eb4a96790bc5156bd9023a64c2d389">c070f579</a>)</li>
            </ul>
          </li>
        </ul>
        <p>Other:</p>
        <ul>
          <li>Fixed potential runtime errors through unhandled promise rejections when
            using <code>Mutation</code> components (<a href="https://gitlab.alvary.io/grnry/react-frontend-admin/-/commit/8342d7ecc1923ac11a4e289171d919c4906ecf63">8342d7ec</a>)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/reaper.md"><b>Reaper</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"><b>0.7.1</b>
        </td>
        <td style="text-align:left">Introduces new Reaper to notify on TTL expired grains.</td>
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
      <td style="text-align:left">0.4.3</td>
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
      <td style="text-align:left"><a href="../installation/zipkin.md">Zipkin Server</a>
      </td>
      <td style="text-align:left">0.6.0 (based on Open Zipkin 2.12)</td>
      <td style="text-align:left">-</td>
    </tr>
  </tbody>
</table>

## Granary 0.7.1 "Kurt" - 2020-02-14

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
      <td style="text-align:left">0.5.0 (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">-</td>
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
      <td style="text-align:left">0.7.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left">0.6.3</td>
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
      <td style="text-align:left"><b>0.7.1</b>
      </td>
      <td style="text-align:left">Fix: DEBUG log output switch working now.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left"><b>0.6.5</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>secure <code>POST /belts/{id}/state</code> (start/stop of a Belt) with <code>belt_edit</code> role</li>
          <li>State of Belt not immediately <code>failed</code> after Belt start up but <code>deploying</code>
          </li>
          <li><code>PUT /belts/{id}</code> (update Belt definition) is now idempotent,
            so no new version in case no configuration updates</li>
          <li>Belt Audit log is written correctly</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left">0.5.3</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left">0.6.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left"><b>0.7.0</b>
      </td>
      <td style="text-align:left">Added support for Profile- &amp; Eventstore Segments based on AWS Aurora.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left">0.4.7 (based on Presto 0.315)</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left">0.6.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left">0.6.0</td>
      <td style="text-align:left">-</td>
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
      <td style="text-align:left"><b>0.4.3</b>
      </td>
      <td style="text-align:left">Fix: Kafka Manager&apos;s Password can be read from a secret now.</td>
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
      <td style="text-align:left">0.6.0 (based on Open Zipkin 2.12)</td>
      <td style="text-align:left">-</td>
    </tr>
  </tbody>
</table>

## Granary 0.7 "Kurt" - 2020-01-10

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
      <td style="text-align:left">0.5.0 (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">-</td>
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
      <td style="text-align:left"><b>0.7.0</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Sessionizing SCDF App added</li>
          <li>JSON Parsing helper method for Scriptable Transform added</li>
          <li>Splitting of arrays as output of Scriptable Transform supported</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left"><b>0.6.3</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Filter for Event Types added to /events and /events/{correlation-id}</li>
          <li>Improved timerange queries</li>
          <li>Harvester and Event Type part of return value</li>
        </ul>
      </td>
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
      <td style="text-align:left"><b>0.7.0</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Belt Framework supports sessionized array of events as input</li>
          <li>messages are now produced to kafka topic with correlation_id as key</li>
          <li>update methods are now chainable</li>
          <li>callback function can now return [] or None without causing an exception</li>
          <li>new Profile Update operations set_max and set_min (incl. History) added</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left"><b>0.6.4</b>
      </td>
      <td style="text-align:left">Fix: totalCount is shown correctly when using search param</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left"><b>0.5.3</b>
      </td>
      <td style="text-align:left">New Profile Update operations set_max and set_min (incl. History) added</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left"><b>0.6.0</b>
      </td>
      <td style="text-align:left">
        <ul>
          <li>Hook runtime for IAM on Profile level added</li>
          <li>GET /profiles returns list of Profile Types</li>
          <li>GET /profiles/{type} returns list of Profiles of a specific type</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left"><b>0.6.0</b>
      </td>
      <td style="text-align:left">Views added to Segments to support IAM on Profile level</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left">0.4.7 (based on Presto 0.315)</td>
      <td style="text-align:left">Prometheus metrics exporter for Segment Store API access added.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left"><b>0.6.0</b>
      </td>
      <td style="text-align:left">see Granary UI</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left"><b>0.6.0</b>
      </td>
      <td style="text-align:left">
        <p><b>Features</b>
        </p>
        <ul>
          <li>Belts
            <ul>
              <li>adds search functionality to the <code>BeltList</code>
              </li>
              <li>improved <code>CopyBeltConfig</code>: Now uses <code>JsonEditor</code> in
                read-only mode, adding syntax highlighting. Can also consume a blacklist
                of fields that should not be shown/copied (used mainly for kubernetes config
                fields)</li>
              <li>add <code>partitionOffsets</code> field to <code>BeltCodeSettings</code> block
                of <em>BeltEdit</em>. Uses <code>JsonEditor</code> with custom schema validation.</li>
            </ul>
          </li>
          <li>Other
            <ul>
              <li>adds new component <code>LoadingIndicator</code> (renders a spinner or pulsating
                dots)</li>
              <li>adds new component <code>List</code>
              </li>
              <li>adds new component <code>JsonEditor</code>
              </li>
              <li>improved <code>Button</code>: Now features <code>ghost</code> and <code>link</code> display
                types and can receive a new <code>loading</code> property which would render
                the <code>LoadingIndicator</code> as button content</li>
              <li>improved <code>Pagination</code>: Now renders a <em>loading</em>  <code>Button</code> and
                offers a render prop for a <em>finish line</em> text that is displayed when
                the button is hidden (when there is no <em>fetchMore</em>)</li>
            </ul>
          </li>
        </ul>
        <p><b>Fixes</b>
        </p>
        <ul>
          <li> <code>isSuccess</code> in async forms is now reliable on subsequent requests
            (as in &quot;save, edit, save again&quot;)</li>
          <li>fixed <code>CardHeader</code> and <code>CardFooter</code> export names</li>
        </ul>
        <p><b>Breaking Changes</b>
        </p>
        <ul>
          <li>refactor <code>useForm</code> custom hook: now returns an object instead
            of an array</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/event-explorer-ui.md"><b>Event Explorer UI</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"><b>0.2.0</b>
        </td>
        <td style="text-align:left">A customizable Event Viewer that allows to browse and filter Event Payload
          for a given Correlation ID. The ordered list of events can be categorized
          / labeled using custom rules and the layout for showing event payloads
          can be customized using templates. The UI is implemented as an Angular
          SPA and redirects to Keycloak for authentication. Only runtime dependency
          are GRNRY REST APIs. The component aims to support use cases around Development
          and Debugging of Granary Apps as well as Interactive Customer Support.</td>
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
      <td style="text-align:left"><a href="../installation/zipkin.md">Zipkin Server</a>
      </td>
      <td style="text-align:left">0.6.0 (based on Open Zipkin 2.12)</td>
      <td style="text-align:left">-</td>
    </tr>
  </tbody>
</table>



