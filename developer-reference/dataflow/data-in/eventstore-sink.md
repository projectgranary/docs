---
description: In this chapter we are introducing the EventStore Sink
---

# EventStore Sink

The EventStore Sink persists data from the outbound topic of the Metadata Extractor into the EventStore. Therefore, it needs to have some meta information about the data to persist.

The EventStore Sink expects that the event headers have been enriched with the following parameters by using the Metadata Extractor:

* grnry-correlation-id&#x20;
* grnry-event-id&#x20;
* grnry-event-type&#x20;
* grnry-event-type-version
* grnry-harvester-name
* grnry-event-timestamp
* grnry-deletion-flag (optional)

The parameters for the EventStore Sink are passed into the Harvester API's helm chart.

### EventStore Batch Sink

**Name**: grnry-eventstore-batch-sink

**Parameters:**  See **** [shared parameters](grnry-components-and-parameters.md)**.**

The parameters for the EventStore&#x20;

Additional configuration parameters:

| Parameter                 | Description                                                                                                                                                                                         | Default        |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| eventstore.tableName      | The table where the data will be inserted into.                                                                                                                                                     | `"eventstore"` |
| eventstore.batchSize      | The batch size after which messages will be inserted into postgres.                                                                                                                                 | `"100"`        |
| eventstore.batchTimeout   | Inserts any messages in batch into postgres if timeout in ms is reached before batch reached `batchSize`.                                                                                           | `"1000"`       |
| eventstore.ttlGracePeriod | Grace period until a deleted grain will be reaped eventually after receiving a tombstone event. Must be a valid ISO\_8601 Duration representation in basic format and thus may not contain hyphens. | `"P5D"`        |

**Source Parameters:**

For the EventStore Sink, the standard jdbc sink is used. For applicable parameters, you may find the configuration here:



{% embed url="https://github.com/spring-cloud-stream-app-starters/jdbc/blob/master/spring-cloud-starter-stream-sink-jdbc/README.adoc" %}
Parameters for the EventStore Sink
{% endembed %}

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
        eventstore.ttlGracePeriod: "PT30S"
        eventstore.batchSize: "100"
        eventstore.batchTimeout: "1000"
```

#### Deletion Flag

In case the `grnry-deletion-flag` is set to true, the Eventstore Batch Sink will add a value to the `ttl` column of the Eventstore table for all (already existing) events specified by `event_type` and `correlation_id` __ (and optional `event_id`) . The `ttl` value will be calculated as the total time-to-live, adding the Event Type's `ttl` and the persister's `ttlGracePeriod` (see above) and subtracting time that has already passed since event creation.

#### Error Handling

In an error case the Sink will fail and restart. The Batch Sink will retry the failed batch as it does not know which message in the batch failed.

### EventStore Sink (Deprecated)

In case of missing Headers, the EventStore Sink will fail and produce the respective entries into the dead letter queue.

**Name:** grnry-eventstore-pg

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

In addition, you will need to fill the following parameter.

```
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

****
