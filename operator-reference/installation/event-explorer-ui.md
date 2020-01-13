# Event Explorer UI

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters: [https://gitlab.alvary.io/grnry/angular-frontend-event/tree/master/helm](https://gitlab.alvary.io/grnry/angular-frontend-event/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/grnry-event-ui \
    --name grnry-event-ui \
    --version <version> \
    -f ./grnry-event-ui-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-event-ui
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-event-ui \
    grnry-stable/grnry-event-ui \
    --version <version> \
    -f ./grnry-event-ui-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-event-ui
```

