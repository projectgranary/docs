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

* [Granary 1.0 Aretha](https://docs.grnry.io/v/1.0-aretha/operator-reference/granary-release-notes)
* [Granary 0.9 Marie](https://docs.grnry.io/v/0.9-marie/operator-reference/granary-release-notes)
* [Granary 0.8 Lemmy](https://docs.grnry.io/v/0.8-lemmy/operator-reference/granary-release-notes)
* [Granary 0.7 Kurt](https://docs.grnry.io/v/0.7-kurt/operator-reference/granary-release-notes)
* [Granary 0.6 Freddie](https://docs.grnry.io/v/0.6-freddie/operator-reference/granary-release-notes)
* [Granary 0.5 Amy](https://docs.grnry.io/v/0.5-amy/operator-reference/granary-release-notes)
* [Granary 0.4 Jimi](https://docs.grnry.io/v/0.4-jimi/operator-reference/granary-release-notes)



## Granary 1.1.0 "Cliff" - 2021-05-28

In this new release we focused on deleting data points from Profile- and Event Store to achieve GDPR compliance. The Profile Store Reaper now features a time-to-notification mode where one can trigger a notification on profiles independent of deleting data. Furthermore, data pipeline elements can now be deleted in Granary UI and Kafka.



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
      <td style="text-align:left"><b>1.1.0</b> (based on Snowplow v0.15.0)</td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add parameter <code>routingHeader</code> that allows to route incoming messages
            to different Kafka topics based on a HTTP header</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/harvester-api/">Harvester API</a>
      </td>
      <td style="text-align:left"><b>1.1.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add Event Type type <code>ttn</code> for notification Event Types</li>
          <li>Delete related Kafka topics upon deletion of a Harvester / Event Type</li>
          <li>Surpress creation of Event Store Persisters for Event Type types <code>ttn</code> and <code>ttl</code>
          </li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Event Types cannot be deleted if they are referenced by a belt</li>
          <li>mTLS setup for connection to Belt-API fixed</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/spring-cloud-data-flow.md">Spring Cloud Dataflow Server and Skipper</a>
      </td>
      <td style="text-align:left"><b>1.1.0</b> (based on Server 2.2.3.RELEASE and Skipper 2.1.4.RELEASE)</td>
      <td
      style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add <code>initContainer</code>to Kubernetes deployments for creating database
            schemas <code>skipper</code> and <code>dataflow</code>
          </li>
        </ul>
        <p></p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/harvester-api/getting-started.md">Spring Cloud Data Flow Apps</a>
      </td>
      <td style="text-align:left"><b>1.1.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add JMS Sink</li>
          <li>Add <code>DeletionEvent</code> wrapper class to Scritable Transform Processor</li>
          <li>Add deletion header SpEL expression to Metadata Extractor Processor</li>
          <li>Add deletion header handling to Batch Event Store Sink</li>
          <li>Add FlyWay for Event Store database table migrations to Batch Event Store
            Sink</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/event-store-api.md">Event Store API</a>
      </td>
      <td style="text-align:left"><b>1.0.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Enable database connection pooling</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/event-feeder.md">Event Feeder</a>
      </td>
      <td style="text-align:left"><b>1.0.1</b>
      </td>
      <td style="text-align:left"><em>internal changes in build and security scanning pipeline only</em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../../developer-reference/dataflow/belt-extractor.md">Belt Extractor</a>
      </td>
      <td style="text-align:left"><b>2.0.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>BREAKING CHANGE: signature of <code>set_value</code> changed: notification
            time <code>ttn</code> is now the 4th argument where <code>ttl</code> was before, <code>ttl</code> is
            now the last argument</li>
          <li>Add parameters to <code>profileClient</code> class to determine the time-out
            and exception handling in case Profile Store API is not responsive</li>
          <li>Add <code>eventstoreClient</code> class to retrieve raw events from Event
            Store API</li>
          <li>Add environment configuration <code>PROFILE_PASS_ON_EXCEPTION</code> to
            skip if profile- and evenstore clients receive a return code other than <code>200</code>
          </li>
          <li>Add class <code>Database</code> to Belt library for writing directly to
            a custom database from within the belt</li>
          <li>Add possibility to define <code>Generic Updates</code> as return value of
            Belt function as extension to <code>Profile Updates</code>.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/belt-api.md">Belt API</a>
      </td>
      <td style="text-align:left"><b>1.1.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add <code>image</code> and <code>imagePullSecret</code> to Belt model</li>
          <li>Delete Belt&apos;s DLQ Kafka topic upon delete request</li>
          <li>Adjust Belt&apos;s consumer group naming to prevent duplicate reassignment
            in Kafka</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/profile-updater.md">Profile Updater</a>
      </td>
      <td style="text-align:left"><b>1.1.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add new profile update operations:
            <ul>
              <li><code>_delete_with_history</code>
              </li>
              <li><code>_set_ttl</code>
              </li>
              <li><code>_set_ttn</code>
              </li>
            </ul>
          </li>
          <li>Add FlyWay for Profile Store database table</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Prevent duplicate primary key exception in all <code>_with_history</code> update
            operations</li>
          <li>Optimize SQL query to validate the profile type</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/profile-store-api.md">Profile Store API</a>
      </td>
      <td style="text-align:left"><b>1.1.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>add new <code>ttn</code> column to Profile payload</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/with-helm/segment-creation-api.md">Segment Management API</a>
      </td>
      <td style="text-align:left"><b>1.1.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add <code>initContainer</code> to Kubernetes deployment for creating database
            schema <code>segment</code>
          </li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Use empty string for users with missing email</li>
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
      <td style="text-align:left"><b>1.1.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add <code>DB_PRESERVE_VIEWS</code> configuration parameter that translates
            to <code>CREATE OR REPLACE</code> instead of dropping and re-creating the
            segment</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/segment-store-api.md">Segment Store API</a>
      </td>
      <td style="text-align:left"><b>1.0.1 (based on Presto 0.339)</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Add <code>extra.JVMArgs</code> configuration parameter to Coordinator and
            Worker</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/graphql-api.md">GraphQL API</a>
      </td>
      <td style="text-align:left"><b>1.1.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add delete methods to all Granary APIs</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>introduce <code>maxHttpHeaderSize</code> parameter to configure the max
            allowed HTTP size in byts</li>
          <li>Fix memory leak in connections to Kafka broker for Event browser</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/granary-ui.md">Granary UI</a>
      </td>
      <td style="text-align:left"><b>1.1.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Complete redesign of Granary UI according to new Design System</li>
          <li>Replace code editor with code highlighter in read-only views</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Not send empty arrays as source type config</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/reaper.md">Reaper</a>
      </td>
      <td style="text-align:left"><b>1.1.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:
          <br />
        </p>
        <ul>
          <li>Add <code>ttn</code> mode to emit notification events</li>
          <li>Add flag to switch between <code>ttl</code> and <code>ttn</code> mode</li>
        </ul>
        <p>Fixes:</p>
        <ul>
          <li>Set default <code>eventTypeRetentionMs</code> to four days</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/with-helm/eventstore-reaper.md"><b>Event Store Reaper</b></a>
      </td>
      <td style="text-align:left"><b>1.1.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Initial release of Event Store Reaper that deletes <code>ttl</code>-expired
            raw events from Event Store and notifies about soon to be deleted raw events</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;<a href="../installation/with-helm/central-deletion-belt.md"><b>Central Deletion Belt</b></a>&lt;b&gt;&lt;/b&gt;</td>
      <td
      style="text-align:left"><b>1.1.0</b>
        </td>
        <td style="text-align:left">
          <p>Features:</p>
          <ul>
            <li>Initial release of Central Deletion Belt that automatically deletes <code>ttl</code>-expired
              profiles from Profile Store</li>
          </ul>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/grapp-importer.md">Granary GrApp Importer</a>
      </td>
      <td style="text-align:left"><b>1.0.1</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Skip starting of an Event Type&apos;s persister by default if type is
            not <code>data_in</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/granary-base-deployment.md">Granary Base Deployment</a>
      </td>
      <td style="text-align:left"><b>1.1.0</b>
      </td>
      <td style="text-align:left">
        <p>Features:</p>
        <ul>
          <li>Add <code>ttn</code> related roles to default realm.json Keycloak setup</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://github.com/bitnami/charts/tree/master/bitnami/postgresql">PostgreSQL</a>
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
      <td style="text-align:left">.-</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/kafka-manager.md">Kafka Manager</a>
      </td>
      <td style="text-align:left"><b>1.0.1</b>
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
      <td style="text-align:left"><b>1.0.1</b>
      </td>
      <td style="text-align:left">
        <p>Fixes:</p>
        <ul>
          <li>Kafka Profile Updater chart shows meaningful charts now</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="../installation/with-helm/zipkin.md">Zipkin Server</a>
      </td>
      <td style="text-align:left">1.0.0 (based on Open Zipkin 2.12)</td>
      <td style="text-align:left">-</td>
    </tr>
  </tbody>
</table>

