# GraphQL API

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/graphql-api/tree/master/helm](https://gitlab.alvary.io/grnry/graphql-api/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/graphql-api \
    --name graphql-api \
    --version <version> \
    -f ./graphql-api-values.yaml
```

Status check and further instructions:

```text
$ helm status graphql-api
```

Upgrade Helm Chart: 

```text
$ helm upgrade graphql-api \
    grnry-stable/graphql-api \
    --version <version> \
    -f ./graphql-api-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge graphql-api
```

