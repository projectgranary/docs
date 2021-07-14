# GrApp Importer

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-grapp-importer/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-stable/grnry-grapp-importer \
    --name grnry-grapp-importer \
    --version <version> \
    -f ./grnry-grapp-importer-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-grapp-importer
```

Upgrade Helm Chart:

```text
$ helm upgrade grnry-grapp-importer \
    grnry-stable/grnry-grapp-importer \
    --version <version> \
    -f ./grnry-grapp-importer-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-grapp-importer
```

