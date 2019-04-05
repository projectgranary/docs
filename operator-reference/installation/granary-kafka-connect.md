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

