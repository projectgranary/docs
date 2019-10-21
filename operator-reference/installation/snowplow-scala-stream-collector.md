# Snowplow Scala Stream Collector

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/docker-snowplow/tree/master/helm](https://gitlab.alvary.io/grnry/docker-snowplow/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/snowplow-scala-stream-collector \
    --name grnry-snowplow-scala-stream-collector \
    --version <version> \
    -f ./snowplow-ssc-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-snowplow-scala-stream-collector 
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-snowplow-scala-stream-collector \
    grnry-stable/snowplow-scala-stream-collector \ 
    --version <version> \
    -f ./snowplow-ssc-values.yaml
```

Tear down Helm Chart: 

```text
$ helm delete --purge grnry-snowplow-scala-stream-collector
```

