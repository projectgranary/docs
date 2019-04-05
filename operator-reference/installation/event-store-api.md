# Event Store API

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/eventstore-api/tree/master/helm](https://gitlab.alvary.io/grnry/eventstore-api/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/eventstore-api \
    --name grnry-eventstore-api \
    --version <version> \
    -f ./grnry-eventstore-api-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-eventstore-api
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-eventstore-api \
    grnry-stable/eventstore-api \
    --version <version> \
    -f ./grnry-eventstore-api-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-eventstore-api
```

