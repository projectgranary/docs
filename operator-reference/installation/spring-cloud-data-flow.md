# Spring Cloud Data Flow

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/scdf-apps/tree/master/helm](https://gitlab.alvary.io/grnry/scdf-apps/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/spring-cloud-data-flow \
    --name grnry-scdf \
    --version <version> \
    -f ./grnry-scdf-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-scdf
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-scdf \
    grnry-stable/spring-cloud-data-flow \
    --version <version> \
    -f ./grnry-scdf-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-scdf
```

