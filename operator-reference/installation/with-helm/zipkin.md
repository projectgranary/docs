# Zipkin

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-docker-zipkin/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-stable/zipkin \
    --name zipkin \
    --version <version> \
    -f ./zipkin-values.yaml
```

Status check and further instructions:

```text
$ helm status zipkin
```

Upgrade Helm Chart:

```text
$ helm upgrade zipkin \
    grnry-stable/zipkin \
    --version <version> \
    -f ./zipkin-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete zipkin
```

