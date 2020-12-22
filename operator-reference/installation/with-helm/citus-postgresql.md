# Citus PostgreSQL

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-citus) for full documentation of all parameters.
{% endhint %}

{% hint style="warning" %}
Deprecated in Granary 0.9. The [Bitnami Postgres Helm Chart](https://github.com/bitnami/charts/tree/master/bitnami/postgresql) must be used in upcoming versions. 
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-oss/citus-postgresql \
    --name grnry-citus-postgresql \
    --version <version> \
    -f ./grnry-citus-postgresql-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-citus-postgresql
```

Upgrade Helm Chart:

```text
$ helm upgrade grnry-citus-postgresql \
    grnry-oss/citus-postgresql \
    --version <version> \
    -f ./grnry-citus-postgresql-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-citus-postgresql
```

