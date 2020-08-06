# Segment Management API

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-segment-management-api/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/segment-management-api \
    --name segment-management-api \
    --version <version> \
    -f ./segment-management-api-values.yaml
```

Status check and further instructions:

```text
$ helm status segment-management-api
```

Upgrade Helm Chart: 

```text
$ helm upgrade segment-management-api \
    grnry-stable/segment-management-api \
    --version <version> \
    -f ./segment-management-api-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete segment-management-api
```

