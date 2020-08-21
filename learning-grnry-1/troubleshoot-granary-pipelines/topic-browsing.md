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
        _message AS message,
        from_utf8(element_at(element_at(_headers,'grnry-dlq-exception-message'),1)) AS exception_message,
        from_utf8(element_at(element_at(_headers,'grnry-dlq-exception-stacktrace'),1)) AS stacktrace
FROM "grnry-kafka".default."grnry_harvester_opensky-harvester_dlq" 
LIMIT 5;
```

#### Tracing headers

The tracing information \(see [Tracing Granary](tracing-granary.md#introduction)\) is available in the message headers.

```text
SELECT 
        _message AS message,
        from_utf8(element_at(element_at(_headers,'grnry-correlation-id'),1)) AS correlation_id,
        from_utf8(element_at(element_at(_headers,'X-B3-Sampled'),1)) AS X_B3_Sampled,
        from_utf8(element_at(element_at(_headers,'X-B3-TraceId'),1)) AS X_B3_TraceId,
        from_utf8(element_at(element_at(_headers,'spanTraceId'),1)) AS spanTraceId,
        from_utf8(element_at(element_at(_headers,'X-B3-ParentSpanId'),1)) AS X_B3_ParentSpanId,
        from_utf8(element_at(element_at(_headers,'spanParentSpanId'),1)) AS spanParentSpanId,
        from_utf8(element_at(element_at(_headers,'X-B3-SpanId'),1)) AS X_B3_SpanId,
        from_utf8(element_at(element_at(_headers,'spanId'),1)) AS spanId
FROM "grnry-kafka".default.grnry_data_in_opensky
LIMIT 5;
```

#### Structured Events

For structured events which contain JSON it is possible to select parts of that JSON.

```text
SELECT 
        json_extract_scalar(json_parse(_message), '$.callsign'),
        json_extract_scalar(json_parse(_message), '$.onGround')
FROM "grnry-kafka".default.grnry_data_in_opensky
LIMIT 5;
```

Filtering messages based on Header fields is possible too.

```text
SELECT *
FROM "grnry-kafka".default.grnry_data_in_opensky
WHERE from_utf8(element_at(element_at(_headers,'grnry-correlation-id'),1)) = '"a68d44"'
LIMIT 5;
```



{% hint style="warning" %}
If a schema or table contains a minus sign \( - \) you need to escape the table name:

```text
"grnry-kafka".default."grnry_harvester_opensky-harvester_dlq"
```
{% endhint %}

