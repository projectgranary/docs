# Profile Store API

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/profilestore-api/tree/master/helm](https://gitlab.alvary.io/grnry/profilestore-api/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/profilestore-api \
    --name grnry-profilestore-api \
    --version <version> \
    -f ./grnry-profilestore-api-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-profilestore-api
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-profilestore-api \
    grnry-stable/profilestore-api \
    --version <version> \
    -f ./grnry-profilestore-api-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-profilestore-api
```

