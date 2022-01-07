---
description: On this page, you get the details about the Metadata Extractor
---

# Metadata Extractor

The Metadata Extractor is used to extract these header information from the message:

* grnry-correlation-id&#x20;
* grnry-event-id&#x20;
* grnry-event-type&#x20;
* grnry-event-type-version
* grnry-harvester-name
* grnry-event-timestamp
* grnry-deletion-flag (optional)

The extraction definitions for these parameters are automatically pulled from the corresponding [Event Type](../event-type.md) definition, they will not be set in your medata extractor definition. See below how to reference an Event Type in a Harvester.

**Name**: grnry-data-in-metadata

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

One sample of a metadata extractor is:

```yaml
"metadataExtractor": {
    "app": "grnry-data-in-metadata",
    "version": "latest",
    "deploymentConfiguration": {
        "kubernetes.requests.memory": "512Mi",
        "kubernetes.requests.cpu": "500m",
        "kubernetes.livenessprobedelay": "120",
        "kubernetes.readinessprobedelay": "120",
        "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
        "kubernetes.limits.cpu": "500m",
        "kubernetes.limits.memory": "512Mi",
        "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
        "kubernetes.imagepullpolicy": "Always"
    },
    "appConfiguration": {
        "spring.cloud.stream.kafka.binder.replicationfactor": "3",
        "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
        "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
        "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
        "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
        "spring.cloud.stream.bindings.input.consumer.concurrency": "6"
    }
}
```



### Reference Event Type in Harvester

As their output, Harvester must reference a [data-in event type](../event-type.md#data-in). One sample of a event type reference is:

```json
{
   ...
   "eventType": {
    "name": "my-event-type",
    "version": "latest"
   
  },
  ...
}
```

