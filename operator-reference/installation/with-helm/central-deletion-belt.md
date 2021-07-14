# Central Deletion Belt

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-central-delete-belt/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-stable/central-deletion-belt-importer \
    --name grnry-central-deletion-belt \
    --version <version> \
    -f ./central-deletion-belt-importer-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-central-deletion-belt
```

Upgrade Helm Chart:

```text
$ helm upgrade grnry-central-deletion-belt \
    grnry-stable/central-deletion-belt-importer \
    --version <version> \
    -f ./central-deletion-belt-importer-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-central-deletion-belt
```

