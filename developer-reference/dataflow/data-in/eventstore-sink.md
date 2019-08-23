---
description: In this chapter we are introducing the EventStore Sink.
---

# EventStore Sink

The EventStore Sink persists data from the outbound topic of the Metadata Extractor into the EventStore. Therefore, it needs to have some meta information about the data to persist. If this data was not available, the EventStore Sink will fail and produce the respective entries into the dead letter queue.

The EventStore expects that you have filled the following parameters when using the metadata extractor:

* grnry-correlation-id 
* grnry-event-id 
* grnry-event-type 
* grnry-harvester-name 
* timestamp

**Name:** grnry-eventstore-pg

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

In addition, you will need to fill the following parameter.

```text
eventstore.tableName=yourSchema.yourEventStoreTable
```

This parameter defines the target, where to put the data.

**Source Parameters:**

For the EventStore Sink, the standard jdbc sink is used. For applicable parameters, you may find the configuration here:



{% embed url="https://github.com/spring-cloud-stream-app-starters/jdbc/blob/master/spring-cloud-starter-stream-sink-jdbc/README.adoc" caption="Parameters for the EventStore Sink" %}

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



