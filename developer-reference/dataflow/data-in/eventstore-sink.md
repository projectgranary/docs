---
description: In this chapter we are introducing the EventStore Sink
---

# EventStore Sink

The EventStore Sink persists data from the outbound topic of the Metadata Extractor into the EventStore. Therefore, it needs to have some meta information about the data to persist.

The EventStore expects that the event headers have been enriched with the following parameters by using the metadata extractor:

* grnry-correlation-id 
* grnry-event-id 
* grnry-event-type 
* grnry-event-type-version
* grnry-harvester-name
* grnry-event-timestamp

The parameters for the EventStore Sink are passed into the Harvester API's helm chart.

### EventStore Batch Sink

**Name**: grnry-eventstore-batch-sink

**Parameters:**  See ****[shared parameters](grnry-components-and-parameters.md)**.**

The parameters for the EventStore 

In addition, you will need to fill following parameter.

| Parameter | Description |
| :--- | :--- |
| eventstore.tableName | The table where the data will be inserted into. |
| eventstore.batchSize | The batch size after which messages will be inserted into postgres. |
| eventstore.batchTimeout | Inserts any messages in batch into postgres if timeout in ms is reached before batch reached `batchSize`. |

**Source Parameters:**

For the EventStore Sink, the standard jdbc sink is used. For applicable parameters, you may find the configuration here:



{% embed url="https://github.com/spring-cloud-stream-app-starters/jdbc/blob/master/spring-cloud-starter-stream-sink-jdbc/README.adoc" caption="Parameters for the EventStore Sink" %}

A sample of an EventStore Batch Sink might look like this:

```yaml
eventstores:
  default:
    name: "pg"
  pg:
    persister:
      scdf:
        appName: "grnry-eventstore-batch"
        appVersion: "latest"
      deploymentConfig:  
        kubernetes.imagePullPolicy: "Always"
        kubernetes.limits.cpu: "500m"
        kubernetes.requests.cpu: "500m"
        kubernetes.limits.memory: "512Mi"
        kubernetes.requests.memory: "512Mi"
        kubernetes.livenessProbeDelay: "30"
        kubernetes.livenessProbePeriod: "60"
        kubernetes.livenessProbeTimeout: "10"
        kubernetes.readinessProbeDelay: "30"
        kubernetes.readinessProbePeriod: "60"
        kubernetes.readinessProbeTimeout: "10"
        kubernetes.volumeMounts: "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]"
        kubernetes.volumes: "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-credentials' , defaultMode : '256' }}]"
      appConfig:
        spring.cloud.kubernetes.secrets.paths: /usr/src/app/db-secret
        spring.datasource.password: "${postgresql-password}"
        spring.datasource.username: "${postgresql-username}"
        spring.datasource.url: jdbc:postgresql://grnry-pg:5432/postgres?currentSchema=public
        spring.cloud.stream.bindings.input.consumer.concurrency: "8"
        spring.cloud.stream.kafka.binder.autoCreateTopics: "true"
        spring.cloud.stream.bindings.input.consumer.maxAttempts: "1"
        spring.cloud.stream.kafka.binder.autoAddPartitions: "true"
        spring.cloud.stream.kafka.binder.replicationFactor: "3"
        spring.cloud.stream.bindings.input.consumer.partitioned: "true"
        spring.cloud.stream.kafka.binder.minPartitionCount: "24"
        eventstore.tableName: public.eventstore
        eventstore.batchSize: "100"
        eventstore.batchTimeout: "1000"
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
eventstores:
  default:
    name: "pg"
  pg:
    persister:
      scdf:
        appName: "grnry-eventstore-pg"
        appVersion: "latest"
      deploymentConfig:  
        kubernetes.imagePullPolicy: "Always"
        kubernetes.limits.cpu: "500m"
        kubernetes.requests.cpu: "500m"
        kubernetes.limits.memory: "512Mi"
        kubernetes.requests.memory: "512Mi"
        kubernetes.livenessProbeDelay: "30"
        kubernetes.livenessProbePeriod: "60"
        kubernetes.livenessProbeTimeout: "10"
        kubernetes.readinessProbeDelay: "30"
        kubernetes.readinessProbePeriod: "60"
        kubernetes.readinessProbeTimeout: "10"
        kubernetes.volumeMounts: "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]"
        kubernetes.volumes: "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-credentials' , defaultMode : '256' }}]"
      appConfig:
        spring.cloud.kubernetes.secrets.paths: /usr/src/app/db-secret
        spring.datasource.password: "${postgresql-password}"
        spring.datasource.username: "${postgresql-username}"
        spring.datasource.url: jdbc:postgresql://grnry-pg:5432/postgres?currentSchema=public
        spring.cloud.stream.bindings.input.consumer.concurrency: "8"
        spring.cloud.stream.kafka.binder.autoCreateTopics: "true"
        spring.cloud.stream.bindings.input.consumer.maxAttempts: "1"
        spring.cloud.stream.kafka.binder.autoAddPartitions: "true"
        spring.cloud.stream.kafka.binder.replicationFactor: "3"
        spring.cloud.stream.bindings.input.consumer.partitioned: "true"
        spring.cloud.stream.kafka.binder.minPartitionCount: "24"
        eventstore.tableName: public.eventstore
```

\*\*\*\*

