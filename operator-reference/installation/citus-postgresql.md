# Citus PostgreSQL

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://github.com/Alvary/charts/tree/master/incubator/citus-postgresql](https://github.com/Alvary/charts/tree/master/incubator/citus-postgresql)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/citus-postgresql \
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
    grnry-stable/citus-postgresql \
    --version <version> \
    -f ./grnry-citus-postgresql-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-citus-postgresql
```

