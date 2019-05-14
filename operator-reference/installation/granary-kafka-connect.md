# Granary Kafka Connect

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/kafka-connect/tree/master/helm](https://gitlab.alvary.io/grnry/kafka-connect/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/kafka-connect \
    --name grnry-kafka-connect \
    --version <version> \
    -f ./grnry-kafka-connect-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-kafka-connect
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-kafka-connect \
    grnry-stable/kafka-connect \
    --version <version> \
    -f ./grnry-kafka-connect-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-kafka-connect
```

## Helm Chart to setup Kafka Connectors

The Helm Chart for Kafka Connetors is needed to deploy Harvesters \(e.g. JDBC Harvester\) or the [Event Store Sink](../../developer-reference/dataflow/event-store.md).

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/deployment/tree/master/charts/incubator/kafka-connector](https://gitlab.alvary.io/grnry/deployment/tree/master/charts/incubator/kafka-connector)
{% endhint %}

Install Helm Chart:

```text
$ helm install grnry-incubator/kafka-connectors \
    --name grnry-kafka-connectors \
    -f ./kafka-connectors-values.yaml
```

Status check and further instructions:

```text
helm status grnry-kafka-connectors
```

Upgrade Helm Chart:

```text
$ helm upgrade grnry-kafka-connectors \
    grnry-incubator/kafka-connectors \
    -f <cluster-name>/kafka-connectors-values.yaml
```

Tear down Helm Chart:

```text
helm delete --purge grnry-kafka-connectors
```

