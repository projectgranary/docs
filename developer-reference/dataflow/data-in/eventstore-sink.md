---
description: In this chapter we are introducing the EventStore Sink
---

# EventStore Sink

The EventStore Sink persists data from the outbound topic of the Metadata Extractor into the EventStore. Therefore, it needs to have some meta information about the data to persist.

The EventStore expects that you have filled the following parameters when using the metadata extractor:

* grnry-correlation-id 
* grnry-event-id 
* grnry-event-type 
* grnry-event-type-version
* grnry-harvester-name
* timestamp

### EventStore Batch Sink

**Name**: grnry-eventstore-batch-sink

**Parameters:**  See ****[**shared parameters**](https://app.gitbook.com/@alvary/s/grnry-sd7f6g8sd68sdf7/~/drafts/-LzMXU-S20C9QM2JKl7C/v/0.8-lemmy/developer-reference/dataflow/data-in/grnry-components-and-parameters)**.**

In addition, you will need to fill following parameter.

| Parameter | Description |
| :--- | :--- |
| grnry-eventstore-batch-sink.eventstore.tableName | The table where the data will be inserted into. |
| grnry-eventstore-batch-sink.eventstore.batchSize | The batch size after which messages will be inserted into postgres. |
| grnry-eventstore-batch-sink.eventstore.batchTimeout | Inserts any messages in batch into postgres if timeout in ms is reached before batch reached `batchSize`. |

**Source Parameters:**

For the EventStore Sink, the standard jdbc sink is used. For applicable parameters, you may find the configuration here:



{% embed url="https://github.com/spring-cloud-stream-app-starters/jdbc/blob/master/spring-cloud-starter-stream-sink-jdbc/README.adoc" caption="Parameters for the EventStore Sink" %}

A sample of an EventStore Batch Sink might look like this:

```yaml
"app.grnry-evenstore-batch-sink.spring.cloud.kubernetes.secrets.paths": "/usr/src/app/db-secret",
"app.grnry-evenstore-batch-sink.spring.datasource.password": "${superuser-password}",
"app.grnry-evenstore-batch-sink.spring.datasource.username": "${superuser-username}",
"app.grnry-evenstore-batch-sink.eventstore.tableName": "<schema>.<table>",
"app.grnry-evenstore-batch-sink.spring.datasource.url": "jdbc:postgresql://<URL>:<Port>/postgres?currentSchema=public",
"app.grnry-evenstore-batch-sink.spring.cloud.stream.bindings.input.consumer.concurrency": 3,
"app.grnry-evenstore-batch-sink.spring.cloud.stream.bindings.input.consumer.partitioned": true,
"app.grnry-evenstore-batch-sink.eventstore.batchSize": "100",
"app.grnry-evenstore-batch-sink.eventstore.batchTimeout": "100",
```

In an error case the Sink will fail and restart. The Batch Sink will retry the failed batch as we do not know which message failed.

### EventStore Sink \(Deprecated\)

In case of missing Headers, the EventStore Sink will fail and produce the respective entries into the dead letter queue.

**Name:** grnry-eventstore-pg

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

In addition, you will need to fill the following parameter.

```text
"app.grnry-eventstore-pg.eventstore.tableName=yourSchema.yourEventStoreTable"
```

This parameter defines the target, where to put the data.

A sample for an EventStore Sink might look like this:

```yaml
	"app.grnry-eventstore-pg.spring.cloud.kubernetes.secrets.paths": "/usr/src/app/db-secret",
	"app.grnry-eventstore-pg.spring.datasource.password": "${superuser-password}",
	"app.grnry-eventstore-pg.spring.datasource.username": "${superuser-username}",
	"app.grnry-eventstore-pg.eventstore.tableName": "<schema>.<table>",
	"app.grnry-eventstore-pg.spring.datasource.url": "jdbc:postgresql://<URL>:<Port>/postgres?currentSchema=public",
	"app.grnry-eventstore-pg.spring.cloud.stream.bindings.input.consumer.concurrency": 3,
	"app.grnry-eventstore-pg.spring.cloud.stream.bindings.input.consumer.partitioned": true,
```

\*\*\*\*

