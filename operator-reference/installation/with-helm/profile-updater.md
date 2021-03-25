# Profile Updater

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-kafka-profile-update/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
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
$ helm delete grnry-kafka-profile-update
```

