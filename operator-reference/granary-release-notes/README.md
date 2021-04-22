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

* [Granary 0.9 Marie](https://docs.grnry.io/v/0.9-marie/operator-reference/granary-release-notes)
* [Granary 0.8 Lemmy](https://docs.grnry.io/v/0.8-lemmy/operator-reference/granary-release-notes)
* [Granary 0.7 Kurt](https://docs.grnry.io/v/0.7-kurt/operator-reference/granary-release-notes)
* [Granary 0.6 Freddie](https://docs.grnry.io/v/0.6-freddie/operator-reference/granary-release-notes)
* [Granary 0.5 Amy](https://docs.grnry.io/v/0.5-amy/operator-reference/granary-release-notes)
* [Granary 0.4 Jimi](https://docs.grnry.io/v/0.4-jimi/operator-reference/granary-release-notes)

## Granary 1.0.1 "Aretha" - 2021-04-15

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
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/kubernetes/ingress-nginx">Nginx</a>
      </td>
      <td style="text-align:left">0.41.2</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/snowplow-scala-stream-collector.md">Snowplow Scala Stream Collector API</a>
      </td>
      <td style="text-align:left">1.0.0 (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/harvester-api/">Harvester API</a>
      </td>
      <td style="text-align:left"><b>1.0.2</b>
      </td>
      <td style="text-align:left">Fixes:</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <ul>
          <li>Event Types cannot be deleted if Belts are still present</li>
          <li>Fix mutual TLS setup for connection to Belt API</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/spring-cloud-data-flow.md">Spring Cloud Dataflow Server and Skipper</a>
      </td>
      <td style="text-align:left">1.0.0 (based on Server 2.2.3.RELEASE and Skipper 2.1.4.RELEASE)</td>
      <td
      style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/harvester-api/getting-started.md">Spring Cloud Data Flow Apps</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/event-feeder.md">Event Feeder</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/belt-extractor.md">Belt Extractor</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/with-helm/segment-creation-api.md">Segment Management API</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/segment-manager.md">Segment Manager</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left"><b>1.0.1</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Add default configuration to CITUS_DIST_COL to make the property optional</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left">1.0.0 (based on Presto 0.339)</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left"><b>1.0.1</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Fix memory leak in connections to Kafka broker for Event browser</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/reaper.md">Reaper</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/with-helm/grapp-importer.md">Granary GrApp Importer</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/granary-base-deployment.md">Granary Base Deployment</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="https://github.com/bitnami/charts/tree/master/bitnami/postgresql">PostgreSQL</a>
      </td>
      <td style="text-align:left">11.10.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/confluentinc/cp-helm-charts/tree/master/charts">Confluent Kafka</a>
      </td>
      <td style="text-align:left">5.5.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/confluentinc/cp-helm-charts/tree/master/charts">Confluent Zookeeper</a>
      </td>
      <td style="text-align:left">5.5.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/kafka-manager.md">Kafka Manager</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/helm/charts/tree/master/stable/keycloak">Keycloak</a>
      </td>
      <td style="text-align:left">11.0.3</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/helm/charts/tree/master/stable/grafana">Grafana</a>
      </td>
      <td style="text-align:left">6.6.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/grafana-dashboards.md">Granary Grafana Dashboards</a>
      </td>
      <td style="text-align:left">1.0.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/zipkin.md">Zipkin Server</a>
      </td>
      <td style="text-align:left">1.0.0 (based on Open Zipkin 2.12)</td>
      <td style="text-align:left">-</td>
    </tr>
  </tbody>
</table>

## Granary 1.0.0 "Aretha" - 2021-01-28

In this new release we focused on 

* UI improvements to accelerate pipeline development and data access
* export & import functionality to stage data pipeline across Granary environments
* reducing technical debt through an harmonized API experience and 
* increasing security through upgraded 3rd party application versions.



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
      <td style="text-align:left"><a href="https://github.com/kubernetes/ingress-nginx">Nginx</a>
      </td>
      <td style="text-align:left">0.41.2</td>
      <td style="text-align:left">see <a href="https://github.com/kubernetes/ingress-nginx/blob/master/Changelog.md#0412">Nginx Release Notes</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/snowplow-scala-stream-collector.md">Snowplow Scala Stream Collector API</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b> (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Ingress compatible with Nginx 0.41.2</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/harvester-api/">Harvester API</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add endpoint <code>POST /harvesters/instances/{name}</code>
          </li>
          <li>Add query parameters for import/export to API endpoints</li>
          <li>Add possibility to configure a list of trusted hosts</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li><code>event_type_{type}_edit</code> role required for creating Event Types</li>
          <li>Ingress compatible with Nginx 0.41.2</li>
          <li>Update jackson-databind library from v2.9.x to v2.10.x to decrease high
            number of known vulnerabilities in previous version</li>
          <li>Postgres replaces Citus as default database</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/spring-cloud-data-flow.md">Spring Cloud Dataflow Server and Skipper</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b> (based on Server 2.2.3.RELEASE and Skipper 2.1.4.RELEASE)</td>
      <td
      style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Update jackson-databind library from v2.9.x to v2.10.x to decrease high
            number of known vulnerabilities in previous version</li>
          <li>Keycloak Client Secret can be referenced from a Kubernetes Secret</li>
          <li>Multiple Kafka brokers and Zookeepers nodes can be configured</li>
          <li>Postgres replaces Citus as default database</li>
        </ul>
        <p></p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/harvester-api/getting-started.md">Spring Cloud Data Flow Apps</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add streaming mode to JDBC Source Type</li>
          <li>Add JMS Source Type for IBM MQ</li>
          <li>Add possibility to configure a custom script to batch Event Store Sink</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Fix not working configuration property naming in Kafka Topic Source Type</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add possibility to configure a list of trusted hosts</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Update jackson-databind library from v2.9.x to v2.10.x to decrease high
            number of known vulnerabilities in previous version</li>
          <li>Postgres replaces Citus as default database</li>
          <li>Ingress compatible with Nginx 0.41.2</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/event-feeder.md">Event Feeder</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <p></p>
        <ul>
          <li>Update jackson-databind library from v2.9.x to v2.10.x to decrease high
            number of known vulnerabilities in previous version</li>
          <li>Postgres replaces Citus as default database</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/belt-extractor.md">Belt Extractor</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add a Retry Exception for events that should be processed another time
            in your callback function</li>
          <li>Refactor code base with PIP package &quot;grnry-belt&quot;</li>
          <li>Add validation of callback method&apos;s signature on startup</li>
          <li>Add automated import of Update class in callback function</li>
          <li>Add configuration option for a Profile&apos;s fragements to <code>getProfile</code> method</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add endpoint <code>POST /belts/{id}</code>
          </li>
          <li>Add query parameters for export to API endpoints</li>
          <li>Add possibility to configure a list of trusted headers</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li><code>belt_edit</code> role required for creating Belts</li>
          <li>Ingress compatible with Nginx 0.41.2</li>
          <li>Update jackson-databind library from v2.9.x to v2.10.x to decrease high
            number of known vulnerabilities in previous version</li>
          <li>Postgres replaces Citus as default database</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Postgres replaces Citus as default database</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Remove deprecated endpoints <code>GET /profiles</code> and <code>GET /profiles/{type}</code>; <code>GET /profiles/{type}/{correlationId}</code> remains
            unchanged</li>
          <li>Add possibility to configure a list of trusted hosts</li>
          <li>Enable database connection pooling</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Ingress compatible with Nginx 0.41.2</li>
          <li>Postgres replaces Citus as default database</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/with-helm/segment-creation-api.md">Segment Management API</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add endpoint <code>POST /segments/{id}</code>
          </li>
          <li>Add query parameters for export to API endpoints</li>
          <li><code>segment_job_edit</code> role required for creating Segments</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Ingress compatible with Nginx 0.41.2</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/segment-manager.md">Segment Manager</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>PUT updates Kubernetes Job correctly</li>
          <li>Postgres replaces Citus as default database</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/segment-store/">Segment Table Creator</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Latest view management in persistent segments works correctly now</li>
          <li>Remove mandatory configuration of <code>CITUS_DIST_COL</code> in case PostgreSQL
            is used</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left"><b>1.0.0 (based on Presto 0.339)</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Ingress compatible with Nginx 0.41.2</li>
          <li>Update jackson-databind library from v2.9.x to v2.10.x to decrease high
            number of known vulnerabilities in previous version</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Enable export mode for resource queries</li>
          <li>Add &quot;pinned items&quot; queries for <code>Belts</code> and <code>Harvesters</code>
          </li>
        </ul>
        <p>See <a href="https://github.com/syncier/grnry-graphql-api/releases/tag/1.0.0">full release notes</a>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add resource item pinning to all list views</li>
          <li>Add optional Kibana link to all detail views</li>
          <li>Add download option to all list views and export overlay to all detail
            views</li>
          <li>Hide edit options when necessary roles are missing in Belt and Event Type
            views</li>
          <li>Add recently viewed profiles to the profile search</li>
        </ul>
        <p>See <a href="https://github.com/syncier/grnry-react-frontend-admin/releases/tag/1.0.0">full release notes</a>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/reaper.md">Reaper</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Harmonize configuration of Keycloak interface with other Granary components</li>
          <li>Postgres replaces Citus as default database</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/with-helm/grapp-importer.md"><b>Granary GrApp Importer</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"><b>1.0.0</b>
        </td>
        <td style="text-align:left">
          <p>Features:</p>
          <ul>
            <li>Initial release of Granary App import job</li>
          </ul>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/granary-base-deployment.md">Granary Base Deployment</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Adapt default realm.json to Keycloak 11</li>
          <li>Add roles needed for Event Browser to default realm.json Keycloak setup</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="https://github.com/bitnami/charts/tree/master/bitnami/postgresql"><b>PostgreSQL</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"><b>11.10.0</b>
        </td>
        <td style="text-align:left">See <a href="https://www.postgresql.org/docs/release/11.10/">PostgreSQL release notes</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/confluentinc/cp-helm-charts/tree/master/charts">Confluent Kafka</a>
      </td>
      <td style="text-align:left"><b>5.5.0</b>
      </td>
      <td style="text-align:left">See <a href="https://docs.confluent.io/5.5.0/release-notes/index.html">Confluent Platform release notes</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/confluentinc/cp-helm-charts/tree/master/charts">Confluent Zookeeper</a>
      </td>
      <td style="text-align:left"><b>5.5.0</b>
      </td>
      <td style="text-align:left">See <a href="https://docs.confluent.io/5.5.0/release-notes/index.html">Confluent Platform release notes</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/kafka-manager.md">Kafka Manager</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Separate <code>contextPath</code> for ingress and for application config</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/helm/charts/tree/master/stable/keycloak">Keycloak</a>
      </td>
      <td style="text-align:left"><b>11.0.3</b>
      </td>
      <td style="text-align:left">See <a href="https://www.keycloak.org/docs/latest/release_notes/#keycloak-11-0-0">Keycloak release notes</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/helm/charts/tree/master/stable/grafana">Grafana</a>
      </td>
      <td style="text-align:left">6.6.0</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/grafana-dashboards.md">Granary Grafana Dashboards</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>PostgreSQL chart adopted to Bitnami PostgreSQL deployment</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/zipkin.md">Zipkin Server</a>
      </td>
      <td style="text-align:left"><b>1.0.0</b> (based on Open Zipkin 2.12)</td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Ingress compatible with Nginx 0.41.2</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

