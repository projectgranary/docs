# Granary Kafka



### Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-kafka) for full documentation of all parameters.
{% endhint %}

### Setup

{% hint style="warning" %}
From the Kafka helm chart, Granary 1.2 requires the components Kafka, Zookeeper, Kafka Connect and Kafka Schema Registry. Please, ensure to deploy them all.
{% endhint %}

Install Helm Chart: 

```text
$ helm install grnry-stable/grnry-kafka \
    --name grnry-kafka \
    --version <version> \
    -f ./grnry-kafka-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-kafka
```

Upgrade Helm Chart:

```text
$ helm upgrade grnry-kafka \
    grnry-stable/grnry-kafka \
    --version <version> \
    -f ./grnry-kafka-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-kafka-deployment
```

