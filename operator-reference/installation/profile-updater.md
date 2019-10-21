# Profile Updater

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/kafka-profile-update/tree/master/helm](https://gitlab.alvary.io/grnry/kafka-profile-update/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/kafka-profile-update \
    --name grnry-kafka-profile-update \
    --version <version> \
    -f ./grnry-kafka-profile-update-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-kafka-profile-update
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-kafka-profile-update \
    grnry-stable/kafka-profile-update \
    --version <version> \
    -f ./grnry-kafka-profile-update-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-kafka-profile-update
```

