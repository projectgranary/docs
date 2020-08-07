# GraphQL API

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-graphql-api/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
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
$ helm delete graphql-api
```

