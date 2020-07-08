---
description: >-
  This side provides a description of the common log format across Granary
  components.
---

# Common Log Format

## Example log output <a id="example-log-output"></a>

```text
2020-06-12 10:58:53.004  INFO [harvester-api,,,] 6644 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/harvesters/instances/{harvester-name}],methods=[PUT]}" onto public org.springframework.hateoas.Resource<io.grnry.harvester_api.harvesters.response.HarvesterResponse> io.grnry.harvester_api.harvesters.HarvesterController.updateHarvester(io.grnry.harvester_api.harvesters.requests.HarvesterRequest,java.lang.String)
```

## Description of Log line fragments <a id="description-of-log-line-fragments"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">Example Fragment</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>2020-06-12 10:58:53.004</code>
      </td>
      <td style="text-align:left">Date and Time</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>INFO</code>
      </td>
      <td style="text-align:left">Log Level</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>[harvester-api,,,]</code>
      </td>
      <td style="text-align:left">
        <p>Zipkin tracing information</p>
        <p>[app-name, X-B3-TraceId, X-B3-SpanId, X-Span-Export]</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>6644</code>
      </td>
      <td style="text-align:left">Process ID</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>[ main]</code>
      </td>
      <td style="text-align:left">Thread name</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>s.w.s.m.m.a.RequestMappingHandlerMapping</code>
      </td>
      <td style="text-align:left">Logger name, here short for: org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>Mapped &quot;{[/harvesters/instances/{harvester-name}],methods=[PUT]}&quot; onto public org.sprin...</code>
      </td>
      <td style="text-align:left">actual log message</td>
    </tr>
  </tbody>
</table>

## [ ](https://app.gitbook.com/@alvary/s/grnry-sd7f6g8sd68sdf7/~/diff/drafts/-M9biSNf20E7e8XlPQij/operator-reference/site-reliability/postgresql-citus/@drafts)Scope of Common Log Format

* All API components
* All SCDF applications
* Kafka Profile Updater
* Snowplow Scala Stream Collector

Belt Extractor log format slighty differs due to Python.

