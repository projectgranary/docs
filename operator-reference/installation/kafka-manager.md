# Kafka Manager

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/kafka-manager/tree/master/helm](https://gitlab.alvary.io/grnry/kafka-manager/tree/master/helm)
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
$ helm delete --purge grnry-kafka-manager
```

