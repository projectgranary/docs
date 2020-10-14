# Granary UI

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-react-frontend-admin/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
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
$ helm delete grnry-ui
```

