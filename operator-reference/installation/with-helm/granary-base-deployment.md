# Granary Base Deployment

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-base-deployment/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-stable/grnry-base-deployment \
    --name grnry-base-deployment \
    --version <version> \
    -f ./grnry-base-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-base-deployment
```

Upgrade Helm Chart:

```text
$ helm upgrade grnry-base-deployment \
    grnry-stable/grnry-base-deployment \
    --version <version> \
    -f ./grnry-base-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-base-deployment
```

