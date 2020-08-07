# Spring Cloud Data Flow

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-docker-scdf/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-stable/spring-cloud-data-flow \
    --name grnry-scdf \
    --version <version> \
    -f ./grnry-scdf-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-scdf
```

Upgrade Helm Chart:

```text
$ helm upgrade grnry-scdf \
    grnry-stable/spring-cloud-data-flow \
    --version <version> \
    -f ./grnry-scdf-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-scdf
```

## Registering the GRNRY SCDF apps

There are two ways to register scdf-apps:

1. Use the SCDF API as described here: [https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/\#api-guide](https://docs.spring.io/spring-cloud-dataflow/docs/current/reference/htmlsingle/#api-guide)
2. Use the SCDF visual "Dashboard" available under `<URL to SCDF>/dashboard`

In the following, we are going to describe the usage of the SCDF API via cURL. However, you should know, that everything you do is available from either the API or the dashboard. In fact, the dashboard uses the API internally.

In order to create pipelines with SCDF, it is necessary to register the used components first. Registering the components is telling the systems which building blocks it may use to create pipelines.

Registering a component is done via the command:

```bash
curl -X POST -u <user>:<password> -d \
"uri=<docker-image-location>&metadata-uri=<maven-artifact-location>" \
https://<URL to SCDF>/apps/<type>/<name>/<version>
```

In line 1 we define to create a cURL request using the POST method. After the `-u` parameter, we define the username and password, separated by a colon. The `-d` parameter is used to pass arguments to the URL. The argument is written in line 2 in this case. Here, we specify the location to the docker image built for SCDF and the metadata-jar \(probably located on a maven-compatible repository\). Both parameters need to be specified in a URL encoded format. A sample for the docker URL location can be found here:

```bash
docker%3A%2F%2Fhub.syncier.cloud%3A5000%2Fgrnry%2Fscdf-apps%2Fgrnry-sftp-source%3Alatest
```

Unencoded this reads: `docker://hub.syncier.cloud/grnry/scdf-apps/grnry-sftp-source:latest`.

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

## List of all mandatory scdf-apps

### Batch Persister \(postgres\)

* Type: `sink`
* name: **grnry-eventstore-batch-sink**
* uri: `docker://hub.syncier.cloud/grnry/scdf-apps/grnry-eventstore-batch-sink:<version>`
* metadata-uri: `maven://io.grnry.scdf-apps:grnry-eventstore-batch-sink:jar:metadata:<version>`

### Scriptable transform

* Type: `processor`
* name: **grnry-scriptable-processor**
* uri: `docker://hub.syncier.cloud/grnry/scdf-apps/grnry-scriptable-processor:<version>`
* metadata-uri: `maven://io.grnry.scdf-apps:grnry-scriptable-processor:jar:metadata:<version>`

### Metadata extractor

* Type: `processor`
* name: **grnry-data-in-metadata-processor**
* uri: `docker://hub.syncier.cloud/grnry/scdf-apps/grnry-data-in-metadata-processor:<version>`
* metadata-uri: `maven://io.grnry.scdf-apps:grnry-data-in-metadata-processor:jar:metadata:<version>`

### Sessionizing

* Type: `processor`
* name: **grnry-sessionizing-processor**
* uri: `docker://hub.syncier.cloud/grnry/scdf-apps/grnry-sessionizing-processor:<version>`
* metadata-uri: `maven://io.grnry.scdf-apps:grnry-sessionizing-processor:jar:metadata:<version>`

### Source Types

see [Source Types](../../developer-reference/dataflow/data-in/source-types.md) for a list and description of all available source types and [Source Types](../../learning-grnry-1/data-in/how-to-run-a-harvester/source-types.md) how to register them.

