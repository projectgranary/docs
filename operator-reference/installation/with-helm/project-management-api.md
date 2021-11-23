# Project Management API

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-management-api) for full documentation of all parameters.&#x20;
{% endhint %}

## Setup

Install Helm Chart:&#x20;

```
$ helm install grnry-stable/management-api \
    --name grnry-management-api \
    --version <version> \
    -f ./grnry-management-api-values.yaml
```

Status check and further instructions:&#x20;

```
$ helm status grnry-management-api
```

Upgrade Helm Chart:&#x20;

```
$ helm upgrade grnry-management-api \
    grnry-stable/management-api \
    --version <version> \
    -f ./grnry-management-api-values.yaml
```

Tear down Helm Chart:&#x20;

```
$ helm delete grnry-management-api
```
