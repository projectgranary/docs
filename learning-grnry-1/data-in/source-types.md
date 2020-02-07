---
description: >-
  This page will give you some background information about source types, source
  type packaging and how to make them available for harvesters.
---

# Source Types

## Source type definition

A source type is a packaged application that enables grnry to pull data from 3rd party systems like s3, databases or webservices. The actual implementation is based on the [https://spring.io/projects/spring-cloud-stream](https://spring.io/projects/spring-cloud-stream) framework, which enables developers to write Supplier functions that connects to third party systems and pulls raw data into an harvester so that this raw data can be processed by a custom transformation script to create valid events. 

## Source Type packaging

The source type is a spring cloud stream based java application that is packaged as a docker container and a jar file. Both artefacts need to be published to a repository and they need to be readable by the grnry-installation \(pull secrets etc\). The docker container is used during deployment and will be executed. The jar  will be downloaded once by grnry to parse the exposed configuration parameters. 

{% hint style="info" %}
The docker artifact is mandatory. The jar file is optional, but without it the grnry installation will not be able to parse the configuration properties and can not assist the user while configuring the harvester.
{% endhint %}

## registering a new source type

A grnry source type is defined by

### name

todo

### version

If you create a docker image and upload it to an docker registry you have to assign a tag / version to it. Make sure to use that docker image version as the source type "version" \(case-sensitive\). As it's bound to docker tags, the version has to match the regex `[\w][\w.-]{0,127}`

### docker uri

The docker uri. If the source type is available on docker hub \([https://hub.docker.com/](https://hub.docker.com/)\) you can use a short version of the uri:

`docker:springcloudstream/ftp-source-kafka:2.1.1.RELEASE`

If you need to pull the artifact from a private repo, the url will look like this:

`docker://<hostname>:<port>/grnry/scdf-apps/grnry-s3-source/0.7.0`

### jar url

the metadata jar can be placed on a maven repository:

`maven://org.springframework.cloud.stream.app:ftl-source-kafka:jar:metadata:2.1.1.RELEASE`

{% hint style="info" %}
If your jar is placed on a private maven repository, you would need to add the address and credentials to the scdf helm chart values file before installing grnry.
{% endhint %}

You don't have to upload the jar file to a fully functional maven repository. You just need to make it available via http and then use a `https://your-host/path/to/your.jar` url.

{% hint style="info" %}
The version used in the jar url does not have to match the version of the source type or the version of the docker artifact.
{% endhint %}

TODO - actual rest call - linked when the harvester-api docs are merged

In order to register a new Source Type you need to insert it directly via SQL for now:

```text
INSERT INTO source_types (name, version, description, uri, metadata_uri, deployer_config, app_config)
VALUES
(
    'source-type-name',
    'latest',
    'NAME jdbc source https://host.com/service/service-jdbc-source',
    'docker://host.com/service/service-jdbc-source:latest',
    'maven://org.springframework.cloud.stream.app:jdbc-source-kafka:jar:metadata:2.1.2.RELEASE',
    '{"kubernetes.imagePullPolicy": "Always", "kubernetes.limits.cpu": "400m"}',
    '{"app.service-name.spring.cloud.stream.bindings.output.producer.partitionCount": 12, "app.service-name.spring.cloud.stream.bindings.output.producer.autoAddPartitions": true}'
)
ON conflict DO NOTHING
```

This is just a temporarly solution. At a later stage this will also be possible directly with an API Call.

## registering a new version of an existing source type

a given source type can be registered multiple time using different versions to enable isolation between already running harvesters and new harvesters that want to leverage new functionality \(existing harvesters can be upgraded as well\).

When you are registering an additional version of a source type, you have to make sure that you use the same docker registry and the same artifact \(with a different tag that is identical to the version\). Otherwise the registration will fail.

TODO - actual rest call - linked when the harvester-api docs are merged

