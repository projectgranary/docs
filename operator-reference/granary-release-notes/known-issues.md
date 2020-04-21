---
description: This page lists known issues to the Granary platform.
---

# Granary Known Issues

<table>
  <thead>
    <tr>
      <th style="text-align:left">Granary Component</th>
      <th style="text-align:left">Structured Name</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Version Occurred</th>
      <th style="text-align:left">Version Fixed</th>
      <th style="text-align:left">Ticket</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Profile Updater</td>
      <td style="text-align:left">GKI_2019_0001</td>
      <td style="text-align:left">All profile updates fail after database connection failure.</td>
      <td style="text-align:left">0.4.1</td>
      <td style="text-align:left">not reproducable</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/190">#190</a> (closed)</td>
    </tr>
    <tr>
      <td style="text-align:left">Profile Updater</td>
      <td style="text-align:left">GKI_2019_0002</td>
      <td style="text-align:left">_delete operation deletes only latest grain value but no historic grain
        values.</td>
      <td style="text-align:left">0.4.1</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/182">#182</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Segment Table Creator</td>
      <td style="text-align:left">GKI_2019_0003</td>
      <td style="text-align:left">Creating views on segments breaks the segment creation.</td>
      <td style="text-align:left">0.4.2</td>
      <td style="text-align:left">0.4.3</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/220">#220</a> (closed)</p>
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/219">#219</a> (closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Segment Table Creator</td>
      <td style="text-align:left">GKI_2019_0004</td>
      <td style="text-align:left">Failing segment creation causes resource contention.</td>
      <td style="text-align:left">0.4.2</td>
      <td style="text-align:left">
        <p>0.4.3</p>
        <p>Needs to be fixed in configuration.</p>
      </td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/221">#221</a> (closed)</td>
    </tr>
    <tr>
      <td style="text-align:left"><del>Kafka Connect</del>
      </td>
      <td style="text-align:left"><del>GKI_2019_0005</del>
      </td>
      <td style="text-align:left"><del>JDBC Connector creates more events than present in source table.</del>
      </td>
      <td style="text-align:left"><del>0.4.1</del>
      </td>
      <td style="text-align:left"><del>-</del>
      </td>
      <td style="text-align:left">&lt;del&gt;&lt;/del&gt;<a href="https://gitlab.alvary.io/grnry/scrum/issues/198"><del>#198</del></a><del> </del>
        <a
        href="https://gitlab.alvary.io/grnry/scrum/issues/199"><del>#199</del>
          </a>&lt;del&gt;&lt;/del&gt;</td>
    </tr>
    <tr>
      <td style="text-align:left">Segment Store API</td>
      <td style="text-align:left">GKI_2019_0006</td>
      <td style="text-align:left">Presto sends too many requests too Keycloak.</td>
      <td style="text-align:left">0.4.3</td>
      <td style="text-align:left">0.4.6</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/193">#193</a> (closed)</td>
    </tr>
    <tr>
      <td style="text-align:left">Profile Updater</td>
      <td style="text-align:left">GKI_2019_0007</td>
      <td style="text-align:left">Profile Updater does not process json.dumps result.</td>
      <td style="text-align:left">0.4.1</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/185">#185</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Profile Updater</td>
      <td style="text-align:left">GKI_2019_0008</td>
      <td style="text-align:left">Events with negative timestamp causes Profile Updater to crash loop.</td>
      <td
      style="text-align:left">0.4.1</td>
        <td style="text-align:left">0.4.2</td>
        <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/223">#223</a> (closed)</td>
    </tr>
    <tr>
      <td style="text-align:left">Belt API</td>
      <td style="text-align:left">GKI_2019_0009</td>
      <td style="text-align:left">After deploying a belt, the API returns state &quot;failed&quot; immediately
        even though deployment is still in progress.</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/323">#323</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Profile Updater</td>
      <td style="text-align:left">GKI_2019_0010</td>
      <td style="text-align:left">Profile Updater service throws runtime exception on unhandled errors instead
        of retrying.</td>
      <td style="text-align:left">0.4.4</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/346">#346</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Profile Updater</td>
      <td style="text-align:left">GKI_2019_0011</td>
      <td style="text-align:left">Grain Type Array too easy to disrupt with _set Grain Type Text operation</td>
      <td
      style="text-align:left">0.4.4</td>
        <td style="text-align:left">0.5.0</td>
        <td style="text-align:left">
          <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/284">#284</a>
          </p>
          <p>(closed)</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Event Store API</td>
      <td style="text-align:left">GKI_2019_0012</td>
      <td style="text-align:left">Event Type must not include character &quot;_&quot;</td>
      <td style="text-align:left">0.4.3</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Server and Skipper</td>
      <td style="text-align:left">GKI_2019_0013</td>
      <td style="text-align:left">SCDF API Endpoints only authenticated not authorized.</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">0.5.6</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/355">#355</a>
        </p>
        <p>(closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Belt API</td>
      <td style="text-align:left">GKI_2019_0014</td>
      <td style="text-align:left">GET belt by name not possible.</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">0.5.5</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/359">#359</a>
        </p>
        <p>(closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Granary UI</td>
      <td style="text-align:left">GKI_2019_0015</td>
      <td style="text-align:left">Profile Type not supported in Profile Explorer.</td>
      <td style="text-align:left">0.3.0</td>
      <td style="text-align:left">0.4.0</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/react-frontend-admin/issues/62">react-frontend-admin#62</a>
        </p>
        <p>(closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Granary UI</td>
      <td style="text-align:left">GKI_2019_0016</td>
      <td style="text-align:left">Profile links not supported as documented in profile specification.</td>
      <td
      style="text-align:left">0.3.0</td>
        <td style="text-align:left">0.4.0</td>
        <td style="text-align:left">
          <p><a href="https://gitlab.alvary.io/grnry/graphql-api/issues/17">graphql-api#17</a>
          </p>
          <p>(closed)</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Apps</td>
      <td style="text-align:left">GKI_2019_0017</td>
      <td style="text-align:left">SCDF Event Store Sink fails on unsupported unicode characters.</td>
      <td
      style="text-align:left">0.5.0</td>
        <td style="text-align:left">0.5.5</td>
        <td style="text-align:left">
          <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/354">#354</a>
          </p>
          <p>(closed)</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Apps</td>
      <td style="text-align:left">GKI_2019_0018</td>
      <td style="text-align:left">Snowplow GET Events not transformed correctly by Scriptable Transform</td>
      <td
      style="text-align:left">0.5.0</td>
        <td style="text-align:left">0.6.0</td>
        <td style="text-align:left">
          <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/446">#446</a>
          </p>
          <p>(closed)</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Apps</td>
      <td style="text-align:left">GKI_2019_0019</td>
      <td style="text-align:left">Splitting Arrays in Scriptable Transform to multiple Kafka messages is
        broken.</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">0.7.0</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/431">#431</a>
        </p>
        <p>(closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Belt API</td>
      <td style="text-align:left">GKI_2019_0020</td>
      <td style="text-align:left">partitionOffset set to &quot;null&quot; causes Belt to not start</td>
      <td
      style="text-align:left">0.5.0</td>
        <td style="text-align:left">-</td>
        <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/447">#447</a>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Belt API</td>
      <td style="text-align:left">GKI_2019_0021</td>
      <td style="text-align:left">Belts deployed by Belt API do not contain Prometheus Scrape annotations.</td>
      <td
      style="text-align:left">0.5.0</td>
        <td style="text-align:left">0.6.3</td>
        <td style="text-align:left">
          <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/455">#455</a>
          </p>
          <p>(closed)</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Belt API</td>
      <td style="text-align:left">GKI_2019_0022</td>
      <td style="text-align:left">DELETE /belt/&lt;id&gt; only deletes belt on database but not on Kubernetes.</td>
      <td
      style="text-align:left">0.5.0</td>
        <td style="text-align:left">0.6.3</td>
        <td style="text-align:left">
          <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/413">#413</a>
          </p>
          <p>(closed)</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Belt API</td>
      <td style="text-align:left">GKI_2019_0023</td>
      <td style="text-align:left">Update of Belt fails witout &quot;editor&quot; present.</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/361">#361</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Apps</td>
      <td style="text-align:left">GKI_2019_0024</td>
      <td style="text-align:left">Eventstore Persister does not have a dead letter queue</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/372">#372</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Profile Store API</td>
      <td style="text-align:left">GKI_2019_0025</td>
      <td style="text-align:left">GET /type/&lt;profile-type&gt; returns single grains not profiles</td>
      <td
      style="text-align:left">0.5.0</td>
        <td style="text-align:left">0.6.0</td>
        <td style="text-align:left">
          <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/443">#443</a>
          </p>
          <p>(closed)</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">Belt API</td>
      <td style="text-align:left">GKI_2019_0026</td>
      <td style="text-align:left">Belt updates in the Belt API are not properly logged</td>
      <td style="text-align:left">0.6.0</td>
      <td style="text-align:left">0.6.5</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/493">#493</a>
        </p>
        <p>(closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Belt API</td>
      <td style="text-align:left">GKI_2019_0027</td>
      <td style="text-align:left">Belt leaves consumer group when callback function is in loop or taking
        to long</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/504">#504</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Belt API</td>
      <td style="text-align:left">GKI_2019_0028</td>
      <td style="text-align:left">POST /belts/{id}/state is not secured by a role</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">0.6.5</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/524">#524</a>
        </p>
        <p>(closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Apps</td>
      <td style="text-align:left">GKI_2019_0029</td>
      <td style="text-align:left">Persister did not recover after multiple restarts (&gt;50) because data
        base was not available</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/516">#516</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Belt API</td>
      <td style="text-align:left">GKI_2019_0030</td>
      <td style="text-align:left">Provide a Docker image for Belt Extractor Runtime that supports GRNRY
        processes to be executed as a non-root user</td>
      <td style="text-align:left">0.6.0</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left"><a href="https://gitlab.alvary.io/grnry/scrum/issues/507">#507</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Apps</td>
      <td style="text-align:left">GKI_2020_0001</td>
      <td style="text-align:left">Kafka-Binder resetOffsets only works for concurrency=1</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">0.7.1</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/532">#532</a>
        </p>
        <p>(closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Spring Cloud Data Flow Apps</td>
      <td style="text-align:left">GKI_2020_0002</td>
      <td style="text-align:left">Meta Data Extractor allows empty string as JSON message which causes Persister
        to fail.</td>
      <td style="text-align:left">0.5.0</td>
      <td style="text-align:left">0.7.1</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/623">#623</a>
        </p>
        <p>(closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">all APIs</td>
      <td style="text-align:left">GKI_2020_0003</td>
      <td style="text-align:left">Next Link one last page is broken.</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">all API-component versions starting from Granary 0.7.2</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/581">#581</a>
        </p>
        <p>(closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Event Store API</td>
      <td style="text-align:left">GKI_2020_0004</td>
      <td style="text-align:left">Event Store API returns incomplete event on GET a Specific Event by ID
        and Correlation ID.</td>
      <td style="text-align:left">0.6.0</td>
      <td style="text-align:left">0.6.4</td>
      <td style="text-align:left">
        <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/607">#607</a>
        </p>
        <p>(closed)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Kafka Profile Updater</td>
      <td style="text-align:left">GKI_2020_0005</td>
      <td style="text-align:left">ThrottleControl blocks fast lane topic and batch consumption by creating
        too many consumers to determine the latest real-time topic offset.</td>
      <td
      style="text-align:left">0.5.1</td>
        <td style="text-align:left">0.5.4</td>
        <td style="text-align:left">
          <p><a href="https://gitlab.alvary.io/grnry/scrum/issues/578">#578</a>
          </p>
          <p>(closed)</p>
        </td>
    </tr>
  </tbody>
</table>

