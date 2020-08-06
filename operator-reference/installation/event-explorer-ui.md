# Event Explorer UI

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-angular-frontend-event/tree/master/helm) for full documentation of all parameters.
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
$ helm delete grnry-event-ui
```

