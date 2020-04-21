# Segment Management API

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/segment-management-api/-/tree/master/helm](https://gitlab.alvary.io/grnry/segment-management-api/-/tree/master/helm)
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
$ helm delete --purge segment-management-api
```

