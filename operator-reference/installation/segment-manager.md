# Segment Manager

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-segment-manager/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-stable/segment-manager \
    --name segment-manager \
    --version <version> \
    -f ./segment-manager-values.yaml
```

Status check and further instructions:

```text
$ helm status segment-manager
```

Upgrade Helm Chart:

```text
$ helm upgrade segment-manager \
    grnry-stable/segment-manager \
    --version <version> \
    -f ./segment-manager-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete segment-manager
```

