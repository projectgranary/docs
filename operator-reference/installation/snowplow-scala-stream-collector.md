# Snowplow Scala Stream Collector

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-docker-snowplow/tree/master/helm) for full documentation of all parameters.
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
$ helm delete grnry-snowplow-scala-stream-collector
```

