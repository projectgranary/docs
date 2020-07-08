# Granary UI

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/react-frontend-admin/tree/master/helm](https://gitlab.alvary.io/grnry/react-frontend-admin/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/grnry-ui \
    --name grnry-ui \
    --version <version> \
    -f ./grnry-ui-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-ui
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-ui \
    grnry-stable/grnry-ui \
    --version <version> \
    -f ./grnry-ui-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-ui
```

