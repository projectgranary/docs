---
description: >-
  This side provides a description of the common log format across Granary
  components. As well as a recommended logging architecture.
---

# Common Log Format

## Scope of Common Log Format

All Java-based components log with the [format introduced below](common-log-format.md#sample-log-output). In more detail, these include:

* All API components
* All SCDF applications
* Kafka Profile Updater
* Snowplow Scala Stream Collector

Belt Extractor log format slighty differs due to the fact that it is a Python application. However, the [Logstash configuration depicted below](common-log-format.md#sample-logstash-configuration) works for Belt Extractor logs, too.

{% hint style="warning" %}
Currently not covered by the Common Log Format are GraphQL API logs.
{% endhint %}

### Sample log output

```text
2020-06-12 10:58:53.004  INFO [harvester-api,,,] 6644 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/harvesters/instances/{harvester-name}],methods=[PUT]}" onto public org.springframework.hateoas.Resource<io.grnry.harvester_api.harvesters.response.HarvesterResponse> io.grnry.harvester_api.harvesters.HarvesterController.updateHarvester(io.grnry.harvester_api.harvesters.requests.HarvesterRequest,java.lang.String)
```

### Description of Log line fragments

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

## Recommended Logging Architecture

### Log Aggregation

Using a node-level logging agent is the most common and encouraged approach for a Kubernetes cluster. It creates only one agent per node, and it doesn't require any changes to the applications running on the node. However, node-level logging _only works for applications' standard output and standard error_.

For example you could use Filebeat as a logging agent reading from your log files on each node and pushing these logs to Logstash where you can filter the logs and extract certain fields from log lines before sending them to Elasticsearch.

![](../../.gitbook/assets/image%20%2844%29.png)

### Sample Filebeat configuration

This is a sample configuration that makes filebeat read from the node's log files, concatenate multiline logs and push to Logstash.

{% code title="filebeat-values.yaml" %}
```text
# Allows you to add any config files in /usr/share/filebeat
# such as filebeat.yml
filebeatConfig:
  filebeat.yml: |
    filebeat.inputs:
    - type: container
      paths:
        - /var/log/containers/*.log
      # Lines that do not start with a date will be added to the previous line
      multiline:
        # date pattern
        pattern: '^[0-9]{4}-[0-9]{2}-[0-9]{2}'
        # match all lines that do NOT start with a date
        negate: true
        # add matching lines to the PREVIOUS line that does not match
        match: after
      processors:
      - add_kubernetes_metadata:
          host: ${NODE_NAME}
          matchers:
          - logs_path:
              logs_path: "/var/log/containers/"
    # output to logstash instance
    output.logstash:
      hosts: ["logstash-logstash:5044"]

```
{% endcode %}

### Sample Logstash configuration

This is a sample configuration allowing Logstash extract the log fragments specified in the common log format [above](common-log-format.md#description-of-log-line-fragments) and push the output to Elasticsearch:

{% code title="logstash-values.yaml" %}
```text
persistence:
  enabled: true

logstashConfig:
  logstash.yml: |
    http.host: 0.0.0.0
    xpack.monitoring.enabled: false
logstashPipeline:
  uptime.conf: |
    input { 
      beats { 
        port => 5044     
      } 
    }
    filter {
      grok {
        match => { "message" => "%{TIMESTAMP_ISO8601:timestamp} *%{LOGLEVEL:level} \[%{DATA:application},%{DATA:X-B3-TraceId},%{DATA:X-B3-SpanId},%{DATA:X-Span-Export}] %{DATA:pid} --- *\[%{DATA:thread}] %{DATA:class} *: %{GREEDYDATA:log-message}" }
        match => { "message" => "\[%{TIMESTAMP_ISO8601:timestamp}\] %{LOGLEVEL:loglevel} \[(?<logger>[^\]]+)\] %{GREEDYDATA:message}" }
        match => { "message" => "\[%{TIMESTAMP_ISO8601:timestamp}\] %{LOGLEVEL:loglevel} %{GREEDYDATA:message}" }
        match => { "message" => "%{DATESTAMP:timestamp} %{TZ} \[%{POSINT:pid}\] %{GREEDYDATA:message}" }
        match => { "message" => "%{TIME:timestamp} %{LOGLEVEL:loglevel}  \[%{DATA:logger}\] %{GREEDYDATA:message}" }
        match => { "message" => "%{IPORHOST:remote_addr} - %{USERNAME:remote_user} \[%{HTTPDATE:time_local}\] \"%{DATA:request}\" %{INT:status} %{NUMBER:bytes_sent} \"%{DATA:http_referer}\" \"%{DATA:http_user_agent}\"" }
        match => { "message" => "%{TIMESTAMP_ISO8601:timestamp}    %{LOGLEVEL:loglevel}    %{USERNAME:logger}    %{GREEDYDATA:loggerpackage}    %{GREEDYDATA:message}" }
      }
    }
    output { elasticsearch { hosts => ["http://elasticsearch-master:9200"] index => "%{[@metadata][beat]}-cra-%{[@metadata][version]}" } }

service:
  annotations: {}
  type: ClusterIP
  ports:
    - name: beats
      port: 5044
      protocol: TCP
      targetPort: 5044
    - name: http
      port: 8080
      protocol: TCP
      targetPort: 8080
```
{% endcode %}

### Sample ELK stack deployment

Deploy these configurations via helm by running the following commands. Make sure you added the "[elastic](https://github.com/elastic/helm-charts)" repo to your helm repos. 

```text
helm repo add elastic https://helm.elastic.co

helm install --name elasticsearch elastic/elasticsearch \
--set resources.requests.cpu=100m \
--set resources.requests.memory=512Mi \
--set resources.limits.cpu=1000m \
--namespace infra-logging

helm install --name filebeat elastic/filebeat \
--set resources.requests.cpu=100m \
--set resources.requests.memory=200Mi \
--set resources.limits.memory=200Mi \
--set resources.limits.cpu=100m \
--namespace infra-logging \
-f grnry/grnry-projects/development-alvary/filebeat-values.yaml


helm install --name logstash elastic/logstash \
--set resources.requests.cpu=100m \
--set resources.requests.memory=128Mi \
--set resources.limits.memory=1536Mi \
--set resources.limits.cpu=1000m \
--namespace infra-logging \
-f grnry/grnry-projects/development-alvary/logstash-values.yaml


helm install --name kibana elastic/kibana \
--set resources.requests.cpu=100m \
--set resources.requests.memory=128Mi \
--set resources.limits.memory=2Gi \
--set resources.limits.cpu=1000m \
--namespace infra-logging
```

