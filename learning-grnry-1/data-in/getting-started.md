---
description: >-
  This page describes a run-through of creating a SCDF pipeline with built-in
  GRNRY
---

# Getting Started

In order to use SCDF with GRNRY, there are two different approaches:

1. Use the SCDF API as described here: [https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/\#api-guide](https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/#api-guide)
2. Use the SCDF visual "Dashboard" available under `<URL to SCDF>/dashboard`

In the following, we are going to describe the usage of the SCDF API via cURL. However, you should know, that everything you do is available from either the API or the dashboard. In fact, the dashboard uses the API internally.

Now, we are going to walk you through the whole process of creating a data pipeline with GRNRY and SCDF. You will see how easy it is to create a scalable data pipeline via some simple commands. Afterwards, we are going to have a more detailed look at the GRNRY enhancements shipped with SCDF and describe ways to configure your deployments.

{% hint style="info" %}
If you wanted to test SCDF on your local computer, you can use the documentation provided here: [https://dataflow.spring.io/docs/installation/local/docker/](https://dataflow.spring.io/docs/installation/local/docker/)
{% endhint %}

## Registering the GRNRY SCDF apps

In order to create pipelines with SCDF, it is necessary to register the used components first. Registering the components is telling the systems which building blocks it may use to create pipelines.

Registering a component is done via the command:

```bash
curl -X POST -u <user>:<password> -d \
"uri=<docker-image-location>" \
https://<URL to SCDF>/apps/<type>/<name>/<version>
```

In line 1 we define to create a cURL request using the POST method. After the `-u` parameter, we define the username and password, separated by a colon. The `-d` parameter is used to pass arguments to the URL. The argument is written in line 2 in this case. Here, we specify the location to the docker image built for SCDF. The docker location needs to be specified in a URL encoded format. A sample for the URL location can be found here:

```bash
docker%3A%2F%2Fgitlab.alvary.io%3A5000%2Fgrnry%2Fscdf-apps%2Fgrnry-sftp-source%3Alatest
```

Unencoded this reads: `docker://gitlab.alvary.io:5000/grnry/scdf-apps/grnry-sftp-source:latest`. 

In line 3, we specify the URL where we register the application itself. First, we define the URL to SCDF. Afterwards we need to write apps to tell SCDF that this is about the apps in its registry. Now, we define the type of the app. This can be for example `source` , `processor` or `sink`. The parameter name then is defined by you. You can assign any name you like. All the components provided by GRNRY usually start with `grnry`. The parameter version is optional. It is possible in SCDF to register apps with different versions. In this case the version could be e.g. `0.5.0` or `latest`. A full example of the URL cood look like this:

```text
https://this.is.my.scdf.URL/apps/source/grnry-sftp/latest
#https://<URL to SCDF     >/apps/<type>/<name    >/<version>
```

{% hint style="info" %}
The docker image provided above is a pre-built component by GRNRY. It supports encryption out of the box and makes your data safer. We highly recommend that you use the images provided by GRNRY.

If you wanted to register another component, just exchange the URL above. An overview on all available components pre-built by GRNRY is available in the subchapters of [Data-In](../../developer-reference/dataflow/data-in/).
{% endhint %}

Further information about using the API for apps can be found in the SCDF documentation here: [https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/\#resources-registered-applications](https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/#resources-registered-applications)

If your components are already registered, you might just want to list the available ones. There is a documentation for this available here: [https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/\#resources-app-registry-list](https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/#resources-app-registry-list)

After registering all the components you need, you can start creating pipelines, which we are going to do in the following paragraphs.

## Creating a data pipeline

Creating a data pipeline consists of several steps:

1. Register the data pipeline
2. Deploy the data pipeline
3. Update/Rollback the data pipeline

### Registering the stream definition

Let us start by registering a stream application. The corresponding API guide can be found here: [https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/\#api-guide-resources-stream-definitions](https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/#api-guide-resources-stream-definitions). 

Registering a stream application is done using the following command:

```bash
curl <URL to SCDF>/streams/definitions?deploy=false \
 -d "name=<stream_name>&definition=<stream_definition>" \
 -u <user>:<password> -X POST
```

To define a stream, we need to define `streams/definitions` as path after the SCDF host. In addition we define as a parameter `deploy=false`. Succeeding the `-d` statement, we write the content of the stream definition. At first, we define the name with an `=` syntax. `<stream_name>` should then be replaced by the name you choose for your stream e.g. `harvester_sftp_adobe_case`. Afterwards we define the stream itself, separated by an ampersand `&`. The stream definition can look like this:

```text
grnry-sftp | grnry-scriptable | grnry-data-in-metadata > :grnry_data_in_sftp-all
```

The stream definition is written in a domain specific language, closely related to the UNIX pipeline commands. The `|` defines a sequence of statements executed in order and in our case the `>` defines writing to or reading from a Kafka topic. In the stream definition itself, you use the name of our first step, the registering of GRNRY components. This means, that we use the names of the components in this definition. In this case the colon in front of grnry\_data\_in\_sftp-all defines that the Kafka Topic is used as a sink here.

{% hint style="info" %}
If you want to know more about the stream definition and the DSL have a look here [https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/\#spring-cloud-dataflow-stream-intro-dsl](https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/#spring-cloud-dataflow-stream-intro-dsl) and here [https://dataflow.spring.io/docs/feature-guides/streams/taps/](https://dataflow.spring.io/docs/feature-guides/streams/taps/)
{% endhint %}

This whole stream definition registered the stream with SCDF. As a next step, it is necessary to deploy the pipeline. There, we define additional configuration settings, which we are going to have a look at on page [The different GRNRY components and their prameters in detail](../../developer-reference/dataflow/data-in/). In the chapter mentioned in the link, you will get to know all the different parameters for the source types, the scriptable-transformation and the grnry-data-in-metadata.

Now, let us take a step back and inspect what we actually did there.

### The data-in pipeline in GRNRY

As you have seen in the above sample, we have a threefold definition of the pipeline. The fourth step is the Kafka topic used as an input for belts. The more abstract definition of the pipeline is:

```text
source | transform | metadata_extractor > :json_meta
```

{% hint style="info" %}
This should be familiar to you from the introduction to this chapter. If you have not read it yet, read it now: [Data-In](./). 
{% endhint %}

In our sample the source is an instantiation of an SFTP source, where there is a grnry component for it, the transform step has been registered as grnry-scriptable and the metadata extractor step is registered as grnry-data-in-metadata.

### Deploying the data-in pipeline

As we have defined the data-in pipeline now, we need to deploy it in SCDF. In this command, we are going to write lots of different configuration parameters, such as code to transform the data or the definition of the event\_type.

This call to SCDF deploys the data-in pipeline \(do not be overwhelmed by it, in later sections we are explaining everything ;-\) \):

```text
curl <URL to SCDF>/streams/deployments/<stream_name> -i -X POST \
 -H "Content-Type: application/json" -u <user>:<password> \
 -d "{""app.grnry-sftp.sftp.factory.username"":""<user>"",\
  ""app.grnry-sftp.sftp.factory.password"":""<password>"",\
  ""app.grnry-sftp.sftp.remote-dir"":""./upload"",\
  ""app.grnry-sftp.sftp.filename-pattern"":""*.input"",\
  ""app.grnry-sftp.sftp.trigger.fixed-delay"": 20,\
  ""app.grnry-sftp.file.consumer.mode"": ""lines"",\
  ""app.grnry-sftp.sftp.factory.allow-unknown-keys"": true,\
  ""app.grnry-sftp.sftp.delete-remote-files"": true,\
  ""app.grnry-sftp.sftp.factory.host"": ""<hostname>"",\
  ""app.grnry-sftp.grnry.harvesterName"":""<harvester_name>"",\
  ""app.grnry-sftp.grnry.eventTypeName"":""<event_name>"",\
  ""app.grnry-sftp.spring.cloud.stream.bindings.output.producer.partitionCount"": ""3"",\
  ""app.grnry-scriptable.management.endpoints.web.exposure.include"": ""prometheus,info,health"",\
  ""app.grnry-scriptable.management.metrics.export.prometheus.enabled"": true,\
  ""app.grnry-scriptable.scriptable-transformer.language"": ""groovy"",\
  ""app.grnry-scriptable.scriptable-transformer.script"": ""import groovy.json.JsonOutput\\nclass Vertrag {\\n    String kd_nr\\n    String v_nr\\n    String text\\n}\\ndef splittedRow = (new String(payload)).split(',')\\ndef c = new Vertrag()\\nc.kd_nr = splittedRow[0]\\nc.v_nr = splittedRow[1]\\nc.text = splittedRow[2]\\nreturn JsonOutput.toJson(c)"",\
  ""app.grnry-scriptable.spring.cloud.stream.kafka.binder.autoCreateTopics"" : true,\
  ""app.grnry-scriptable.spring.cloud.stream.bindings.input.consumer.concurrency"": 3,\
  ""app.grnry-scriptable.spring.cloud.stream.bindings.input.consumer.partitioned"": true,\
  ""app.grnry-scriptable.spring.cloud.stream.bindings.output.producer.partitionCount"": 3,\
  ""app.grnry-scriptable.spring.cloud.stream.bindings.output.producer.autoAddPartitions"": true,\
  ""app.grnry-scriptable.spring.cloud.stream.kafka.bindings.input.consumer.resetOffsets"": false,\
  ""app.grnry-scriptable.spring.cloud.stream.kafka.bindings.input.consumer.startOffset"": ""latest"",\
  ""app.grnry-scriptable.grnry.harvesterName"" : ""<harvester_name>"",\
  ""app.grnry-scriptable.grnry.eventTypeName"" : ""<event_name>"",\
  ""app.grnry-data-in-metadata.metadata.correlationIdExpression"": ""#safeJsonPath(payload, 'kd_nr')?:'NO_CORRELATION_ID'"",\
  ""app.grnry-data-in-metadata.metadata.eventIdExpression"": ""#randomUUID()"",\
  ""app.grnry-data-in-metadata.metadata.eventTypeName"": ""${grnry.eventTypeName}"",\
  ""app.grnry-data-in-metadata.metadata.harvesterName"": ""${grnry.harvesterName}"",\
  ""app.grnry-data-in-metadata.spring.cloud.stream.bindings.input.consumer.concurrency"": 3,\
  ""app.grnry-data-in-metadata.spring.cloud.stream.bindings.input.consumer.partitioned"": true,\
  ""app.grnry-data-in-metadata.spring.cloud.stream.kafka.binder.autoAddPartitions"" : true,\
  ""app.grnry-data-in-metadata.grnry.harvesterName"" : ""<harvester_name>"" ,\
  ""app.grnry-data-in-metadata.grnry.eventTypeName"" : ""<event_name>"" ,\
  ""deployer.*.kubernetes.imagePullPolicy"": ""Always"",\
  ""deployer.*.kubernetes.limits.cpu"": ""400m"",\
  ""deployer.*.kubernetes.limits.memory"": ""512Mi"",\
  ""deployer.*.kubernetes.requests.cpu"": ""400m"",\
  ""deployer.*.kubernetes.requests.memory"": ""512Mi"",\
  ""deployer.*.kubernetes.livenessProbeDelay"":""120"",\
  ""deployer.*.kubernetes.readinessProbeDelay"":""120"",\
  ""deployer.*.kubernetes.volumeMounts"": ""[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }]"",\
  ""deployer.*.kubernetes.volumes"": ""[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]"" }"
```

{% hint style="info" %}
Due to technical limitations in the [Event Store API](../../developer-reference/api-reference/event-store-api.md), the `eventTypeName` may not contain "`_`".
{% endhint %}

The different parameters in the JSON are build in the following way:

```text
<target>.<registered_component>.<parameter>=<target_value>
```

In this case `target` is `app`, the `registered_component` is `grnry-sftp` \(for lines 3-14\) and the parameter is e.g. `sftp.remote-dir`.

{% hint style="warning" %}
A full overview of the different configuration parameters can be found here: [The different GRNRY components and their parameters in detail](../../developer-reference/dataflow/data-in/).

In this sample, we focus on understanding the general structure of the API requests.
{% endhint %}

As you can see in the sample above, there are different sections of parameters. The first part of parameters starts with `app`, the second starts with `deployer`. Everything starting with `app` relates to the configuration of the application/stream. The information for `deployer` relate to the operations part and e.g. define where to find the secrets for data access.

Most of this definition for deployment should be familar to you by now. Let us have a look at two things. First, the `<stream_name>` as defined in line 1 is of course the same as for the definition you did in previous chapters. Second, after the `-d` parameter, we now define a JSON string with the configuration parameters. There you should put all the different configuration parameters that your application needs. 

After sending this statement to the SCDF API, SCDF will automatically deploy three containers:

![The created pods in a Kubernetes Cluster](../../.gitbook/assets/grafik%20%287%29.png)

{% hint style="info" %}
`harvester-sftp-all-std` reflects our stream name. We recommend, that you choose a similar naming convention, to keep up with your deployments.
{% endhint %}

{% hint style="info" %}
If you face any issues here, it is worth to have a look into the logs of your pods.
{% endhint %}

### Updating/Rolling back data-in deployments

It is, of course, possible to update or rollback the data-in deployments you have done. A detailed instruction on how to do so can be found in the SCDF documentation: [https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/\#api-guide-resources-stream-deployment-update](https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/#api-guide-resources-stream-deployment-update) However, in general the process is similar to the previous paragraph.

## Creating an eventstore sink

As of now, we have a fully functional pipeline for SCDF running and belts can process data based on the output topic. However, in our introductory picture, we have seen, there is an [Event Store](../../developer-reference/dataflow/event-store.md) as well. This EventStore should be filled with the raw events before being processed by belts. Hence, we need to define a pipeline to the EventStore, our sink, as well.

In order to create such a sink, it is necessary to register the pipeline for it and define what it should do. A general guideline for the stream name is: `persister_<event_type>`. Hence, the pipeline looks very similar to this one:

```text
:grnry_data_in_sftp-all > grnry-eventstore-pg
```

This is a very short pipeline and gets data from the Kafka Topic grnry\_data\_in\_sftp-all and writes these data to grnry-eventstore-pg. grnry-eventstore-pg is another GRNRY component that is used to persist the event data into our persistent storage.

To create this pipeline, we have the following steps:

1. Register the stream.
2. Deploy the stream with parameters.

Above, we have already seen, what we need to do, to achieve this. Here, are the steps once again in short:

```bash
curl <URL to SCDF>/streams/definitions?deploy=false \
 -d "name=<stream_name>&definition=<stream_definition>" \
 -u <user>:<password> -X POST
```

In this sample &lt;stream\_definition&gt; is something like:

```text
:grnry_data_in_<event_type> > grnry-eventstore-pg
```

Afterwards you deploy the stream, which you can do by executing:

```bash
curl <URL to SCDF>/streams/deployments/<stream_name> -i -X POST \
 -H "Content-Type: application/json" -u <user>:<password> \
 -d "{
	""app.grnry-eventstore-pg.spring.cloud.kubernetes.secrets.paths"": ""/usr/src/app/db-secret"",
	""app.grnry-eventstore-pg.spring.datasource.password"": ""${superuser-password}"",
	""app.grnry-eventstore-pg.spring.datasource.username"": ""${superuser-username}"",
	""app.grnry-eventstore-pg.eventstore.tableName"": ""public.eventstore"",
	""app.grnry-eventstore-pg.spring.datasource.url"": ""jdbc:postgresql://grnry-pg-citus:5432/postgres?currentSchema=public"",
	""app.grnry-eventstore-pg.spring.cloud.stream.bindings.input.consumer.concurrency"": 3,
	""app.grnry-eventstore-pg.spring.cloud.stream.bindings.input.consumer.partitioned"": true,
	""deployer.*.kubernetes.imagePullPolicy"": ""Always"",
	""deployer.*.kubernetes.limits.cpu"": ""500m"",
	""deployer.*.kubernetes.limits.memory"": "512Mi"",
	""deployer.*.kubernetes.requests.cpu"": "500m"",
	""deployer.*.kubernetes.requests.memory"": "512Mi"",
	""deployer.*.kubernetes.livenessProbeDelay"": "120"",
	""deployer.*.kubernetes.readinessProbeDelay"": "120"",
	""deployer.*.kubernetes.volumeMounts"": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]"",
	""deployer.*.kubernetes.volumes"": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]""
}
```

As a result, your stream should be up and running and you should see that from the Kafka topic :grnry\_data\_in\_&lt;event\_type&gt; the events are persisted to the Postgres.

## Bindings Parameters

Using GRNRY you need to configure your consumer and producer bindings. As a consumer you define how you want to handle incoming data. As a producer you define where you write to. Properties applying to this pattern are for example:

```yaml
"<prefix>.spring.cloud.stream.kafka.binder.autoCreateTopics" : true,\
"<prefix>.spring.cloud.stream.bindings.input.consumer.concurrency": 3,\
"<prefix>.spring.cloud.stream.bindings.input.consumer.partitioned": true,\
"<prefix>.spring.cloud.stream.bindings.output.producer.partitionCount": 3,\
"<prefix>.spring.cloud.stream.bindings.output.producer.autoAddPartitions": true,\
"<prefix>.spring.cloud.stream.kafka.bindings.input.consumer.resetOffsets": false,\
"<prefix>.spring.cloud.stream.kafka.bindings.input.consumer.startOffset": "latest" 
```

An overview of the different available and applicable parameters can be found here:

{% embed url="https://cloud.spring.io/spring-cloud-static/spring-cloud-stream/2.1.2.RELEASE/single/spring-cloud-stream.html\#binding-properties" %}

### Dead letter queue

A special kind of bindings in GRNRY are the dead letter queues. In a dead letter queue, everything that cannot be processed by the SCDF pipeline is written. For a full overview on the dead letter queue, refer to [the documentation](../../developer-reference/dataflow/data-in/grnry-components-and-parameters.md#dead-letter-queues). We leave it out here, as we think this is an advanced topic.

## Deployment parameters

As you have seen above, it is possible to define deployment properties for the different applications. The deployment properties are described in more detail in the following link:

{% embed url="https://docs.spring.io/spring-cloud-dataflow-server-kubernetes/docs/current/reference/htmlsingle/\#\_application\_and\_server\_properties" %}

Please note, that if you deploy on a different environment than Kubernetes, there are implementations for many different environments provided with SCDF.

A sample for deployment parameters could look like this:

```yaml
  "deployer.*.kubernetes.imagePullPolicy": "Always",
  "deployer.*.kubernetes.limits.cpu": "400m",
  "deployer.*.kubernetes.limits.memory": "512Mi",
  "deployer.*.kubernetes.requests.cpu": "400m",
  "deployer.*.kubernetes.requests.memory": "512Mi",
  "deployer.*.kubernetes.livenessProbeDelay":"120",
  "deployer.*.kubernetes.readinessProbeDelay":"120",
  "deployer.*.kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }]",
  "deployer.*.kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]"
```

The star `*` refers to all entries, otherwise you may write the name of a specific application, such as `grnry-sftp`.

Especially the bottom two entries are important, they allow you to mount a shared secret for encryption. The standard deployer properties to mount the kafka message de-/encryption keys need to be extended to also mount the secret containing the database credentials. With this secret accessible to the sink implementation, you can use spring-cloud-kubernetes functionality described here: [https://github.com/spring-cloud/spring-cloud-kubernetes\#secrets-propertysource](https://github.com/spring-cloud/spring-cloud-kubernetes#secrets-propertysource). 

Example: 

```yaml
kubernetes.volumeMounts=[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }] 
kubernetes.volumes=[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-dev-secret' , defaultMode : '256' }}]

spring.cloud.
deployer.*.kubernetes.limits.cpu
kubernetes.secrets.paths=/usr/src/app/db-secret 
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

