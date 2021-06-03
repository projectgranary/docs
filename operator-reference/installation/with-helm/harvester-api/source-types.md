---
description: >-
  This page will give you some background information about source types, source
  type packaging and how to make them available to harvesters.
---

# Source Types

## Source Type Definition

A Source Type entity is a combination of a source app, an app-version and some configuration. A source app is a packaged application that enables grnry to pull data from 3rd party systems like s3, databases or webservices. The actual implementation is based on the [https://spring.io/projects/spring-cloud-stream](https://spring.io/projects/spring-cloud-stream) framework, which enables developers to write supplier functions that connect to third party systems and pull raw data into a harvester in a way that this raw data can be processed by a custom transformation script to create equivalent json documents \(events\).

## Source Type Packaging

The source app is a spring cloud stream based java application that is packaged as a docker container and a jar file. Both artefacts need to be published to a repository and they need to be readable by the Granary-installation \(pull secrets etc\). The docker container is used during deployment and will be executed. The jar will be downloaded once by Granary to parse the exposed configuration parameters.

{% hint style="info" %}
The docker artifact is mandatory. The jar file is optional, but without it the Granary installation will not be able to parse the configuration properties and cannot assist the user while configuring the harvester.
{% endhint %}

## Registering a New Source Type

A Granary source type entity has the following attributes:

| Attribute | Description |
| :--- | :--- |
| **`name`** | Name of the source app. Needs to be registered with SCDF beforehand. |
| **`version`** | Version of the source app. Needs to be registered with SCDF. |
| **`uri`** | The docker repository uri. |
| **`metadata_uri`** | metadata jar location |
| **`deployer_config`** | A map containing deployer \(e.g. kubernetes\) specific configuration. |
| **`app_config`** | A map containing app specific configuration. |
| **`description`** | Description of this specific source type. _Optional_, default is `""`. |

### name

Name of the source app.

### version

If you create a docker image and upload it to an docker registry you have to assign a tag \(i.e., version\) to it. Make sure to use that docker image version as the source type `version` \(case-sensitive\). As it is bound to docker tags, the version has to match the regex `[\w][\w.-]{0,127}`.

### uri

The docker repository URI. If the source type is available on docker hub \([https://hub.docker.com/](https://hub.docker.com/)\) you can use a short version of the URI:

`docker:springcloudstream/ftp-source-kafka:2.1.1.RELEASE`

If you need to pull the artifact from a private repo, the URI will look like this:

`docker://<hostname>:<port>/grnry/scdf-apps/grnry-s3-source/0.7.0`

### metadata\_uri

the metadata jar can be placed on a maven repository:

`maven://org.springframework.cloud.stream.app:ftl-source-kafka:jar:metadata:2.1.1.RELEASE`

Example for an actual Granary SCDF Source Type:

`maven://io.grnry.scdf-apps:grnry-jdbc-source:jar:metadata:0.8.0`

{% hint style="info" %}
If your jar is placed on a private maven repository, you would need to add the address and credentials to the scdf helm chart values file before installing Granary.
{% endhint %}

You don't have to upload the jar file to a fully functional maven repository. You just need to make it available via http and then use a `https://your-host/path/to/your.jar` url.

{% hint style="info" %}
The version used in the jar url does not have to match the version of the source type or the version of the docker artifact.
{% endhint %}

### Create a New Source Type Entity

In order to create a new source type, you will have to register the source app with scdf before you insert the source type directly via SQL for now. _This is just a temporary solution. At a later stage this will also be possible directly with an API Call._

```text
INSERT INTO source_types (
    name, 
    version, 
    description, 
    uri, 
    metadata_uri, 
    deployer_config, 
    app_config
)
VALUES (
    'jdbc',
    'latest',
    'NAME jdbc source https://host.com/service/service-jdbc-source',
    'docker://host.com/service/service-jdbc-source:latest',
    'maven://org.springframework.cloud.stream.app:jdbc-source-kafka:jar:metadata:2.1.2.RELEASE',
    '{"kubernetes.imagePullPolicy": "Always", "kubernetes.limits.cpu": "400m"}',
    '{"spring.cloud.stream.bindings.output.producer.partitionCount": 12, "spring.cloud.stream.bindings.output.producer.autoAddPartitions": true}'
)
ON conflict DO NOTHING
```

and register the app manually in scdf:

```text
curl -X POST -u <user>:<password> -d \
"uri=docker://host.com/service/service-jdbc-source:latest&metadata-uri=maven://org.springframework.cloud.stream.app:jdbc-source-kafka:jar:metadata:2.1.2.RELEASE" \
https://scdf-host/apps/source/jdbc/latest
```

### View Existing Source Types \(also see [API Docs](../../../../developer-reference/api-reference/harvester-api/source-type-endpoints.md#get-all-source-types)\)

#### Example Call GET All Source Types

```bash
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -o response.json \
 https://grnry-host/harvesters/source-types
```

{% code title="response.json" %}
```text
{
    "sourceTypes": [
        {
            "name": "grnry-http",
            "_links": {
                "self": {
                    "href": "https://grnry-host/harvesters/source-types/grnry-http?offset=0&pagesize=20&expand="
                }
            }
        },
        {
            "name": "grnry-jdbc",
            "_links": {
                "self": {
                    "href": "https://grnry-host/harvesters/source-types/grnry-jdbc?offset=0&pagesize=20&expand="
                }
            }
        },
        {
            "name": "grnry-sftp",
            "_links": {
                "self": {
                    "href": "https://grnry-host/harvesters/source-types/grnry-sftp?offset=0&pagesize=20&expand="
                }
            }
        },
        {
            "name": "grnry-topic",
            "_links": {
                "self": {
                    "href": "https://grnry-host/harvesters/source-types/grnry-topic?offset=0&pagesize=20&expand="
                }
            }
        }
    ],
    "totalCount": 4,
    "_links": {
        "self": {
            "href": "https://grnry-host/harvesters/source-types?search=&offset=0&pagesize=20&expand="
        }
    }
}
```
{% endcode %}

#### Example Call GET specific Source Type

```bash
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -o response.json \
 https://grnry-host/harvesters/source-types/grnry-http/0.6.0
```

{% code title="response.json" %}
```text
{
    "sourceTypes": [
        {
            "name": "grnry-http",
            "version": "0.6.0",
            "description": "GRNRY http source",
            "metadataUri": "maven://org.springframework.cloud.stream.app:http-source-kafka:jar:metadata:2.1.1.RELEASE",
            "uri": "docker://hub.syncier.cloud/grnry/scdf-apps/grnry-http-source:0.6.0",
            "appConfig": {
                "app.grnry-scriptable.spring.cloud.stream.bindings.output.producer.autoAddPartitions": true,
                "app.grnry-scriptable.spring.cloud.stream.bindings.output.producer.partitionCount": 12
            },
            "deployerConfig": {
                "kubernetes.imagePullPolicy": "Always",
                "kubernetes.limits.cpu": "400m",
                "kubernetes.limits.memory": "512Mi",
                "kubernetes.livenessProbeDelay": "120",
                "kubernetes.readinessProbeDelay": "120",
                "kubernetes.requests.cpu": "400m",
                "kubernetes.requests.memory": "512Mi",
                "kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly: 'true' }]",
                "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]"
            },
            "_links": {
                "self": {
                    "href": "https://grnry-host/harvesters/source-types/grnry-http/0.6.0"
                }
            }
        }
    ],
    "totalCount": 1,
    "_links": {
        "self": {
            "href": "https://grnry-host/harvesters/source-types?search=grnry-http&offset=0&pagesize=20&expand="
        }
    }
}
```
{% endcode %}

## Registering a Source Type With a New Version of an Existing Source App

A source app can be registered multiple times with different image tags. There will be a separate source type entity for each version of the registered app. This ensures isolation between already running harvester instances that point to older versions and new harvesters that leverage new functionality. Running harvester instances can still be upgraded explicitly.

When registering an additional version of a source app, it is prerequisite to use the same docker registry and the same artifact \(with a different tag, i.e., version\). Otherwise the registration will fail.

