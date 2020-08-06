# Kafka Manager

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-kafka-manager/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/kafka-manager \
    --name grnry-kafka-manager \
    --version <version> \
    -f ./grnry-kafka-manager-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-kafka-manager
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-kafka-manager \
    grnry-stable/kafka-manager \
    --version <version> \
    -f ./grnry-kafka-manager-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-kafka-manager
```

