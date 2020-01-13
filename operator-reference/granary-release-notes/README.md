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

