# Zipkin

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/docker-zipkin/tree/master/helm](https://gitlab.alvary.io/grnry/docker-zipkin/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
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
$ helm delete --purge zipkin
```

