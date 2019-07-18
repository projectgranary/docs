---
description: Installs SCDF Server and Skipper
---

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
    --name spring-cloud-data-flow \
    --version <version> \
    -f ./spring-cloud-data-flow-values.yaml
```

Status check and further instructions:

```text
$ helm status spring-cloud-data-flow
```

Upgrade Helm Chart: 

```text
$ helm upgrade spring-cloud-data-flow \
    grnry-stable/spring-cloud-data-flow \
    --version <version> \
    -f ./spring-cloud-data-flow-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge spring-cloud-data-flow
```

