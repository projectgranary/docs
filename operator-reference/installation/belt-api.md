# Belt API

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-belt-api/tree/master/helm) for full documentation of all parameters.
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
$ helm delete grnry-belt-api
```

