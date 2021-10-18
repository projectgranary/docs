---
description: >-
  On this page, available Granary Harvester components and their corresponding
  parameters are described.
---

# Shared parameters

## Kubernetes Deployment

The parameters within `deploymentConfiguration` control the Kubernetes deployment of a Granary Harvester component:

```javascript
"sourceType" : {
    "deploymentConfiguration": {
        "kubernetes.imagePullPolicy": "IfNotPresent",
        "kubernetes.limits.cpu": "300m",
        "kubernetes.limits.memory": "512Mi",
        "kubernetes.requests.cpu": "300m",
        "kubernetes.requests.memory": "512Mi",
        "kubernetes.livenessProbeDelay":"120",
        "kubernetes.readinessProbeDelay":"120"
    },
    ...
 }
```

### Encryption

Granary Harvester components support symmetric encryption. Therefore the encryption secret containing the key pair needs to be mounted to Granary Harvester components like so:

```javascript
"sourceType" : {
    "deploymentConfiguration": {
        "kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }]",
        "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]"
    },
    ...
 }
```

To deactivate the encryption, do not pass in the mounted secrets as shown above and overwrite the (de-)serializer within `appConfiguration` of each Granary Harvester component:

```javascript
"transform" : {
    "appConfiguration" : {
        "spring.cloud.stream.kafka.bindings.input.consumer.configuration.value.deserializer":"org.apache.kafka.common.serialization.ByteArrayDeserializer",
        "spring.cloud.stream.kafka.bindings.output.producer.configuration.value.serializer":"org.apache.kafka.common.serialization.ByteArraySerializer"
    },
    ...
 }
```

## Dead letter queues

In GRNRY, so called _dead letter queues _are automatically created for each Harvester. These dead letter queues are used to receive all the data that could not be processed correctly by the [transform](scriptable-transform.md) or [metadata extractor](metadata-extractor.md) steps. They are the output channels for errors.

### When is something written to the Dead Letter Queue?

The result is, that whenever there is an error in your transform or metadata extractor step, the data is sent to the dead letter queue. There you get the description of the error and the **original** payload. The original payload refers to the data you have received as input for the transform step, meaning the input to your harvester after the source type. By doing so, we make sure, you do not drop the original data of your request and can reprocess it, if wanted.

## Tracing

All Granary Harvester components support tracing with OpenZipkin, an open-sourced distributed tracing system. If you want to disable tracing, please add the following optional parameter:

```javascript
"transform" : {
    "appConfiguration" : {
        "spring.sleuth.enabled": false
    },
    ...
 }
```

## Using kubernetes secrets as configuration

In some cases you do not want to configure your Harvester using plaintext properties. This could be the case if you are using the SFTP Source and have to authenticate at your server. To pass in the credentials using a kubernetes secret you need to do the following:

Instead of passing the credentials to the source as plaintext, use environment variables and make them reference th kubernetes secret:

```javascript
 "sourceType" : {
    ...
    "configuration" : {
      "sftp.factory.username": "${SECRET_SFTP_USER}",
      "sftp.factory.password": "${SECRET_SFTP_PASSWORD}",
      ...
    },
    "deploymentConfiguration": {
      "kubernetes.secretKeyRefs": "[{envVarName: 'SECRET_SFTP_USER', secretName: 'sftp-secret', dataKey: 'user'},{envVarName: 'SECRET_SFTP_PASSWORD', secretName: 'sftp-secret', dataKey: 'password'}]",
      ...
    }
 }
```

