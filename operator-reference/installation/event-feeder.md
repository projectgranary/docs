# Event Feeder

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/event-feeder/tree/master/helm](https://gitlab.alvary.io/grnry/eventstore-api/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/event-feeder \
    --name grnry-event-feeder \
    --version <version> \
    -f ./grnry-event-feeder-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-event-feeder
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-event-feeder \
    grnry-stable/event-feeder \
    --version <version> \
    -f ./grnry-event-feeder-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-event-feeder
```

