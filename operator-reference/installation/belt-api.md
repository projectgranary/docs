# Belt API

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/belt-api/tree/master/helm](https://gitlab.alvary.io/grnry/eventstore-api/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/belt-api \
    --name grnry-belt-api \
    --version <version> \
    -f ./grnry-belt-api-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-belt-api
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-belt-api \
    grnry-stable/belt-api \
    --version <version> \
    -f ./grnry-belt-api-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-belt-api
```

