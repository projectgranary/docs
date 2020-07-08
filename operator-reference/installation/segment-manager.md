# Segment Manager

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/segment-manager/-/tree/master/helm](https://gitlab.alvary.io/grnry/segment-manager/-/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
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
$ helm delete --purge segment-manager
```

