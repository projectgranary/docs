---
description: >-
  This page describes how to use Presto to read the contents of Kafka topics in
  Granary.
---

# Browsing Kafka Topics

There are cases when the application logs and the tracing information are not enough and you need to access a topic directly. For example when you want to reproduce a rarely occurring event or access the DLQ Dead Letter Queue.

### Prerequisits

{% hint style="info" %}
To setup Presto for reading topics you need to enable the Kafka \(see [Segment Store API](../../operator-reference/installation/with-helm/segment-store-api.md#setup)\) Connector and grant proper permissions in Keycloak's "jdbc-api" client to the user who want to query a topic \(see [Granary Access Clients](../../operator-reference/identity-and-access-management/granary-access-clients.md#jdbc-api-a-k-a-segment-store-api)\).
{% endhint %}

### Access topics via Presto

{% hint style="info" %}
You can select a schema, then you don't need to type it in queries again.

```text
USE "grnry-kafka".default;
SHOW TABLES;
```
{% endhint %}

#### Dead Letter Queue

To get your messages from a DLQ and show the corresponding exception messages and stack trace you can use a query like the following.

```text
SELECT 
        from_utf8(element_at(element_at(_headers,'grnry-dlc-exception-message'),1)) AS exception_message,
        from_utf8(element_at(element_at(_headers,'grnry-dlc-exception-stacktrace'),1)) AS stacktrace
FROM "grnry-kafka".default."grnry_harvester_dlq_sunshine-harvester" 
LIMIT 5;
```

| exception\_message | stacktrace |
| :--- | :--- |
| "nested exception is org.springframework.messaging.MessageHandlingException: failed to execute script; nested exception is org.springframework.integration.scripting.ScriptingException: ReferenceError: \"console\" is not defined in  at line number 1; nested exception is javax.script.ScriptException: ReferenceError: \"console\" is not defined in  at line number 1, failedMessage=GenericMessage \[payload=byte\[969\], headers={deliveryAttempt=3, X-B3-ParentSpanId=cb9353cbc045c8a3, kafka\_timestampType=CREATE\_TIME, scst\_partition=13, kafka\_receivedTopic=g-h-sunshine-harvester.src, spanTraceId=472a4913c41bf460, spanId=43585354498c6a26, spanParentSpanId=cb9353cbc045c8a3, nativeHeaders={spanTraceId=\[472a4913c41bf460\], spanId=\[971c8221a5709f9d\], spanParentSpanId=\[229dabc3177fd25f\], spanSampled=\[1\], X-B3-TraceId=\[472a4913c41bf460\], X-B3-SpanId=\[971c8221a5709f9d\], X-B3-ParentSpanId=\[229dabc3177fd25f\], X-B3-Sampled=\[1\]}, kafka\_offset=1, X-B3-SpanId=43585354498c6a26, scst\_nativeHeadersPresent=true, kafka\_consumer=org.apache.kafka.clients.consumer.KafkaConsumer@6d5ae700, X-B3-Sampled=1, X-B3-TraceId=472a4913c41bf460, id=2f48a995-2ae7-724c-06c9-8969e8687af0, spanSampled=1, kafka\_receivedPartitionId=13, contentType=application/json, kafka\_receivedTimestamp=1598339535560, timestamp=1598339538600}\]" | "org.springframework.messaging.MessageHandlingException: nested exception is org.springframework.messaging.MessageHandlingException: failed to execute script; nested exception is org.springframework.integration.scripting.ScriptingException: ReferenceError: \"console\" is not defined in  at line number 1; nested exception is javax.script.ScriptException: ReferenceError: \"console\" is not defined in  at line number 1, failedMessage=GenericMessage \[payload=byte\[969\], headers={deliveryAttempt=3, X-B3-ParentSpanId=cb9353cbc045c8a3, kafka_timestampType=CREATETIME, scstpartition=13, kafka\_receivedTopic=g-h-sunshine-harvester.src, spanTraceId=472a4913c41bf460, spanId=43585354498c6a26, spanParentSpanId=cb9353cbc045c8a3, nativeHeaders={spanTraceId=\[472a4913c41bf460\], spanId=\[971c8221a5709f9d\], spanParentSpanId=\[229dabc3177fd25f\], spanSampled=\[1\], X-B3-TraceId=\[472a4913c41bf460\], X-B3-SpanId=\[971c8221a5709f9d\], X-B3-ParentSpanId=\[229dabc3177fd25f\], X-B3-Sampled=\[1\]}, kafka\_offset=1, X-B3-SpanId=43585354498c6a26, scst\_nativeHeadersPresent=true, kafka\_consumer=org.apache.kafka.clients.consumer.KafkaConsumer@6d5ae700, X-B3-Sampled=1, X-B3-TraceId=472a4913c41bf460, id=2f48a995-2ae7-724c-06c9-8969e8687af0, spanSampled=1, kafka\_receivedPartitionId=13, contentType=application/json, kafka\_receivedTimestamp=1598339535560, timestamp=1598339538600}\], failedMessage=GenericMessage \[payload=byte\[969\], headers={deliveryAttempt=3, X-B3-ParentSpanId=cb9353cbc045c8a3, kafka\_timestampType=CREATE\_TIME, scst\_partition=13, kafka\_receivedTopic=g-h-sunshine-harvester.src, spanTraceId=472a4913c41bf460, spanId=43585354498c6a26, spanParentSpanId=cb9353cbc045c8a3, nativeHeaders={spanTraceId=\[472a4913c41bf460\], spanId=\[971c8221a5709f9d\], spanParentSpanId=\[229dabc3177fd25f\], spanSampled=\[1\], X-B3-TraceId=\[472a4913c41bf460\], X-B3-SpanId=\[971c8221a5709f9d\], X-B3-ParentSpanId=\[229dabc3177fd25f\], X-B3-Sampled=\[1\]}, kafka\_offset=1, X-B3-SpanId=43585354498c6a26, scst\_nativeHeadersPresent=true, kafka\_consumer=org.apache.kafka.clients.consumer.KafkaConsumer@6d5ae700, X-B3-Sampled=1, X-B3-TraceId=472a4913c41bf460, id=2f48a995-2ae7-724c-06c9-8969e8687af0, spanSampled=1, kafka\_receivedPartitionId=13, contentType=application/json, kafka\_receivedTimestamp=1598339535560, timestamp=1598339538600}\]\n\tat org.springframework.integration.handler.MethodInvokingMessageProcessor.processMessage\(MethodInvokingMessageProcessor.java:109\)\n\tat org.springframework.integration.handler.ServiceActivatingHandler.handleRequestMessage\(ServiceActivatingHandler.java:93\)\n\tat org.springframework.integration.handler.AbstractReplyProducingMessageHandler.handleMessageInternal\(AbstractReplyProducingMessageHandler.java:123\)\n\tat org.springframework.integration.handler.AbstractMessageHandler.handleMessage\(AbstractMessageHandler.java:162\)\n\tat org.springframework.integration.dispatcher.AbstractDispatcher.tryOptimizedDispatch\(AbstractDispatcher.java:115\)\n\tat org.springframework.integration.dispatcher.UnicastingDispatcher.doDispatch\(UnicastingDispatcher.java:132\)\n\tat_  |
|  |  |

#### Tracing headers

The tracing information \(see [Tracing Granary](tracing-granary.md#introduction)\) is available in the message headers.

```text
SELECT 
        _message AS message,
        from_utf8(element_at(element_at(_headers,'grnry-correlation-id'),1)) AS correlation_id,
        from_utf8(element_at(element_at(_headers,'X-B3-Sampled'),1)) AS X_B3_Sampled,
        from_utf8(element_at(element_at(_headers,'X-B3-TraceId'),1)) AS X_B3_TraceId,
        from_utf8(element_at(element_at(_headers,'X-B3-ParentSpanId'),1)) AS X_B3_ParentSpanId,
        from_utf8(element_at(element_at(_headers,'X-B3-SpanId'),1)) AS X_B3_SpanId
FROM "grnry-kafka".default.grnry_data_in_opensky
LIMIT 5;
```

| message         | correlation\_id | X\_B3\_Sampled | X\_B3\_TraceId | X\_B3\_ParentSpanId | X\_B3\_SpanId |
| :--- | :--- | :--- | :--- | :--- | :--- |
|  {"setNetworkUserId"**:**true,"headersIterator"**:**\["Host: localhost","X-Request-ID: 40a743eb8f2f58dfb3b708ca503e0107","X-Real-Ip: 93.220.207.61","X-Forwarded-For: 93.220.207.61","X-Forwarded-Host: localhost","X-Forwarded-Port: 443","X-Forwarded-Proto: https","X-Scheme: https","User-Agent: PostmanRuntime/7.26.3","Accept: \*/\*","Cache-Control: no-cache","Postman-Token: 4384b690-2ee4-4e26-9549-80bac67cc819","Accept-Encoding: gzip, deflate, br","Timeout-Access: &lt;function1&gt;","application/json"\],"encoding"**:**"UTF-8","setBody"**:**true,"headersSize"**:**15,"setQuerystring"**:**false,"setPath"**:**true,"refererUri"**:**null,"timestamp"**:**1598007049000,"path"**:**"/com.snowplowanalytics.snowplow/tp2","setSchema"**:**true,"body"**:**"{\n  \"data\": \[\n\t{\n\t  \"correlationId\": \"some\_id\",\n\t  \"filterCriteria\": \"harvester\_criteria\"\n\t}\n \]\n}\n","ipAddress"**:**"93.220.207.61","schema"**:**"iglu:com.snowplowanalytics.snowplow/CollectorPayload/thrift/1-0-0","setHostname"**:**true,"setUserAgent"**:**true,"setTimestamp"**:**true,"setEncoding"**:**true,"setCollector"**:**true,"setRefererUri"**:**false,"hostname"**:**"localhost","setHeaders"**:**true,"querystring"**:**null,"headers"**:**\["Host: localhost","X-Request-ID: 40a743eb8f2f58dfb3b708ca503e0107","X-Real-Ip: 93.220.207.61","X-Forwarded-For: 93.220.207.61","X-Forwarded-Host: localhost","X-Forwarded-Port: 443","X-Forwarded-Proto: https","X-Scheme: https","User-Agent: PostmanRuntime/7.26.3","Accept: \*/\*","Cache-Control: no-cache","Postman-Token: 4384b690-2ee4-4e26-9549-80bac67cc819","Accept-Encoding: gzip, deflate, br","Timeout-Access: &lt;function1&gt;","application/json"\],"contentType"**:**"application/json","setIpAddress"**:**true,"userAgent"**:**"PostmanRuntime/7.26.3","setContentType"**:**true,"collector"**:**"ssc-0.15.0-kafka","networkUserId"**:**"3249466d-3946-4a66-98e6-4c0423d23f11"} | "CorID" | "1" | "5177307fea725fe5"  | "b6a79b036931c1c3"  | "b2ed5190b23c09f3" |

#### Structured Events

For structured events which contain JSON it is possible to select parts of that JSON.

```text
SELECT 
        json_extract_scalar(json_parse(_message), '$.body')
FROM "grnry-kafka".default.grnry_data_in_sunshine
LIMIT 5;
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">_col0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>{</p>
        <p>&quot;data&quot;<b>:</b> [</p>
        <p>{</p>
        <p>&quot;correlationId&quot;: &quot;some_id&quot;,</p>
        <p>&quot;filterCriteria&quot;: &quot;harvester_criteria&quot;</p>
        <p>}</p>
        <p>]</p>
        <p>}</p>
      </td>
    </tr>
  </tbody>
</table>

Filtering messages based on Header fields is possible too.

```text
SELECT _headers
FROM "grnry-kafka".default.grnry_data_in_sunshine
WHERE from_utf8(element_at(element_at(_headers,'grnry-correlation-id'),1)) = '"CorID"'
LIMIT 5;
```

| \_headers |
| :--- |
| {grnry**-**harvester**-**name**=**\[\[B@1fb58234\], grnry**-**event**-**id**=**\[\[B@6717fadc\], deliveryAttempt**=**\[\[B@b8dad8b\], X**-**B3**-**ParentSpanId**=**\[\[B@7984c42f\], scst\_partition**=**\[\[B@4a935d09\], grnry**-**event**-**type**-**version**=**\[\[B@120a8f9d\], spanId**=**\[\[B@740d99a2\], spanTraceId**=**\[\[B@2b779eb0\], spanParentSpanId**=**\[\[B@7bdcf5c2\], nativeHeaders**=**\[\[B@3decfb43\], grnry**-**event**-**type**=**\[\[B@25b4a5d5\], spring\_json\_header\_types**=**\[\[B@32a13dcd\], X**-**B3**-**SpanId**=**\[\[B@2ecfbcbe\], scst\_nativeHeadersPresent**=**\[\[B@74237cc7\], X**-**B3**-**Sampled**=**\[\[B@7439e59b\], X**-**B3**-**TraceId**=**\[\[B@4fcde184\], grnry**-**correlation**-**id**=**\[\[B@1c348e8f\], grnry**-**event**-**timestamp**=**\[\[B@739e1c8a\], spanSampled**=**\[\[B@48b02a5f\], contentType**=**\[\[B@6851330c\]} |

{% hint style="warning" %}
If a schema or table contains a minus sign \( - \) you need to escape the table name:

```text
"grnry-kafka".default."grnry_harvester_opensky-harvester_dlq"
```
{% endhint %}

