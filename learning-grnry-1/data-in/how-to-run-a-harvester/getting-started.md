# Deploy and App Configuration

{% hint style="info" %}
The main audience of this page is not a person that is creating and configuring harvesters. The harvester definition allows to explicitly include all environment specific properties, but the harvester-api can be configured with reasonable defaults for source types , transform and metadata extractor applications. This page should explain and support defining these defaults.
{% endhint %}

## Application Properties

Each component \(source type , transform + metadata extractor\) of a harvester and the eventstore-persister is a spring cloud stream application that allows to be customised.

There are two levels available:

General Spring Cloud Stream configuration properties as described here:

{% embed url="https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/2.2.1.RELEASE/spring-cloud-stream.html\#\_configuration\_options" %}

And kafka-specific configuration properties as described here:

{% embed url="https://cloud.spring.io/spring-cloud-stream-binder-kafka/spring-cloud-stream-binder-kafka.html\#\_configuration\_options" %}

These are the most common options you want to set. It will increase the parallelism in the pod \(number of threads, not pod count\) from 1 to 6 and it also makes sure that the intermediate topics  are created with a proper partition count and replication setting. And it also disables the internal retry mechanism of the application by limiting `maxAttempts` to 1 \(default would be 3 attempts\).

```yaml
"spring.cloud.stream.bindings.input.consumer.concurrency" : "6"


"spring.cloud.stream.kafka.binder.autoCreateTopics" : "true"
"spring.cloud.stream.kafka.binder.autoAddPartitions": "true"
"spring.cloud.stream.kafka.binder.replicationFactor : "3"
"spring.cloud.stream.bindings.input.consumer.partitioned : "true"
"spring.cloud.stream.kafka.binder.minPartitionCount : "24"

"spring.cloud.stream.kafka.binder.consumer.maxAttempts" : "1"
```

## Deployment Properties

Besides the actual application configuration you also need to provide mandatory kubernetes deployment configuration. All supported configuration options are listed here:

{% embed url="https://docs.spring.io/spring-cloud-dataflow-server-kubernetes/docs/current/reference/htmlsingle/\#\_application\_and\_server\_properties" %}

{% hint style="info" %}
Spring Cloud Data Flow itself is platform-agnostic, kubernetes is just one of many deployment options. GRNRY is only supporting kubernetes.
{% endhint %}

A sample for deployment parameters could look like this:

```yaml
  "kubernetes.imagePullPolicy": "Always",
  "kubernetes.limits.cpu": "400m",
  "kubernetes.limits.memory": "512Mi",
  "kubernetes.requests.cpu": "400m",
  "kubernetes.requests.memory": "512Mi",
  "kubernetes.livenessProbeDelay":"120",
  "kubernetes.readinessProbeDelay":"120",
  "kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }]",
  "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]"
```

### Placing Username und Password in Kubernetes secrets

Especially the bottom two entries are important, they allow you to mount a shared secret for encryption. The standard deployer properties to mount the kafka message de-/encryption keys need to be extended to also mount the secret containing the database credentials. With this secret accessible to the sink implementation, you can use spring-cloud-kubernetes functionality described here:

{% embed url="https://github.com/spring-cloud/spring-cloud-kubernetes\#secrets-propertysource" %}

Example for jdbc-source:

harvester: sourceType/deployConfiguration:

```yaml
kubernetes.volumeMounts=[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }] 
kubernetes.volumes=[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-dev-secret' , defaultMode : '256' }}]
```

harvester: sourceType/appConfiguration:

```yaml
kubernetes.secrets.paths=/usr/src/app/db-secret 
```

harvester: sourceType/configuration:

```yaml
spring.datasource.password=${superuser-password} 
spring.datasource.username=${superuser-username}
```

### Recommended parameter settings

As a minimum for the deployment of these applications, we recommend the following settings:

| Parameter | Minimum recommended |
| :--- | :--- |
| limits.kubernetes.cpu | 300m |
| limits.memory | 256Mi |
| requests.cpu | 300m |
| requests.memory | 256Mi |

With these minimally set parameters, you should also increase the liveness probe to 3 minutes \(180s\).

{% hint style="info" %}
Keep in mind, to increase the parameter: 

```text
spring.cloud.stream.bindings.input.consumer.concurrency
```

if you increase the pod's resources as this is the number of threads running the application and could be one of the limiting factors.
{% endhint %}

