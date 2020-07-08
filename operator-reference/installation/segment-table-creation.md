# Segment Table Creation

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/segment-table-creation/tree/master/helm](https://gitlab.alvary.io/grnry/segment-table-creation/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/grnry-segment-creation \
    --name grnry-segment-<segment-name> \
    --version <version> \
    -f ./grnry-segment-<segment-name>-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-segment-<segment-name>
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-segment-<segment-name> \
    grnry-stable/grnry-segment-creation \
    --version <version> \
    -f ./grnry-segment-<segment-name>-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-segment-<segment-name>
```

