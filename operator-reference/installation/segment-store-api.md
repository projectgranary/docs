# Segment Store API

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/presto-access-control/tree/master/helm](https://gitlab.alvary.io/grnry/presto-access-control/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/presto \
    --name grnry-segmentstore-api \
    --version <version> \
    -f ./grnry-segmentstore-api-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-segmentstore-api
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-segmentstore-api \
    grnry-stable/presto \
    --version <version> \
    -f ./grnry-segmentstore-api-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-segmentstore-api
```

### Access to Kafka topics

The Segment Store API can be configured to access encrypted GRNRY Topics. With the following configuration in the "grnry-segmentstore-api-values.yaml" file.

```text
kafka:
  enabled: true
  tables: snowplow,topic1,topic2
```

{% hint style="warning" %}
The topic list is loaded at startup. For a reload the service needs to be restarted!
{% endhint %}

{% hint style="info" %}
If you leave the tables empty a Kafka client is started to fetch ALL topics.
{% endhint %}

