# Harvester API

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-harvester-api/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Prerequisits

The Harvester API is backed by Spring Cloud Data Flow \( [https://dataflow.spring.io/getting-started/](https://dataflow.spring.io/getting-started/) \). Hence it requires a fully-configured [Spring Cloud Data Flow](../spring-cloud-data-flow.md) setup. Including the [registration ](../spring-cloud-data-flow.md#registering-the-grnry-scdf-apps)of [all mandatory SCDF apps](../spring-cloud-data-flow.md#list-of-all-mandatory-scdf-apps). The Harvester API will provide the functionality to create/update/delete [Event Types](../../../learning-grnry-1/data-in/how-to-run-a-harvester/event-types.md), [Source Types](source-types.md) and [Harvesters](../../../learning-grnry-1/data-in/how-to-run-a-harvester/harvesters.md) and also to manage the deployment of these pipelines.

The current state of the Harvester API will do most of the Spring Cloud Data Flow interaction internally, but it's still mandatory to register certain scdf-apps upfront. The Harvester API will validate the availablilty of all mandatory applications on startup and will abort if they are not found.

The Harvester API also provides default settings for Harvesters. Read the subpage [SCDF App and Deployment Configuration](getting-started.md) to learn how to configure the defaults.

## Setup

For default configuration recommendations refer to the "Get Data Into Granary" [Deploy and App Configuration](getting-started.md).

Install Helm Chart:

```text
$ helm install grnry-stable/harvester-api \
    --name grnry-harvester-api \
    --version <version> \
    -f ./grnry-harvester-api-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-harvester-api
```

Upgrade Helm Chart:

```text
$ helm upgrade grnry-harvester-api \
    grnry-stable/harvester-api \
    --version <version> \
    -f ./grnry-harvester-api-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-harvester-api
```

