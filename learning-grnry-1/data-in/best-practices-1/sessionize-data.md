# Sessionize Data

## About Sessionizing <a id="about-sessionizing"></a>

Taken from [https://kafka.apache.org/20/documentation/streams/developer-guide/dsl-api.html\#session-windows](https://kafka.apache.org/20/documentation/streams/developer-guide/dsl-api.html#session-windows):

Session windows are used to aggregate key-based events into so-called _sessions_, the process of which is referred to as _sessionization_. Sessions represent a **period of activity** separated by a defined **gap of inactivity** \(or “idleness”\). Any events processed that fall within the inactivity gap of any existing sessions are merged into the existing sessions. If an event falls outside of the session gap, then a new session will be created.

![](../../../.gitbook/assets/image%20%2818%29.png)

### Example

| User / SessionID | Timestamp |
| :--- | :--- |
| A | 2019-01-10 10:00 |
| A | 2019-01-10 10:13 |
| A | 2019-01-10 10:22  |
| A  | 2019-01-10 11:21 |
| A | 2019-01-10 11:45 |

If you want to group data based on inactivity between events, the output varies based on your inactivity period configuration:

#### Inactivity between events 10 minutes

1. Group: \( 2019-01-10 10:00 \)
2. Group: \(2019-01-10 10:13 , 2019-01-10 10:22 \)
3. Group: \(2019-01-10 11:21\)
4. Group: \(2019-01-10 11:45\)

#### Inactivity between events 15 Minutes:

1. Group: \(2019-01-10 10:00 , 2019-01-10 10:13 , 2019-01-10 10:22\)
2. Group: \(2019-01-10 11:21\)
3. Group: \(2019-01-10 11:45\)

#### Inactivity between events 30 Minutes:

1. Group: \(2019-01-10 10:00 , 2019-01-10 10:13 , 2019-01-10 10:22\)
2. Group: \(2019-01-10 11:21 , 2019-01-10 11:45\)

#### Inactivity between events 60 Minutes:

1. Group: \(2019-01-10 10:00 , 2019-01-10 10:13 , 2019-01-10 10:22, 2019-01-10 11:21 , 2019-01-10 11:45\)

## Create a Harvester With Sessionizing

This section describes how to include the sessionizing processor in [API creation](../../../developer-reference/api-reference/harvester-api.md#create-harvester) of a harvester.

### Registering the SCDF app

{% hint style="info" %}
This section is an addition to [Getting Started](../../../operator-reference/installation/harvester-api/getting-started.md), it will not explain in detail how to create a harvester, but only how to register the necessary application and how to add the additional parameters to a standard \(non-sessionizing\) harvester. 
{% endhint %}

In order to add the additional step to your harvester definition, you need to register an scdf   
application \(`grnry-sessionizing`\) with your scdf instance.

Registering a component is done via the command:

```text
curl -X POST -u <user>:<password> -d \
"uri=<docker-image-location>" \
https://<URL to SCDF>/apps/processor/grnry-sessionizing/<version>
```

using the provided `docker-image-location` and `version`.

### Extending the Harvester Definition

A minimal sessionizing harvester definition needs to set `sessionizing.enabled` to `true` and looks like this:

```text
{
  "displayName": "harvester-1",
  "sourceType": {
    "name": "source-type-1",
    "version": "latest"
  },
  "eventType": {
    "name": "event-type-1",
    "version": "latest"
  },
  "sessionizing": {
    "enabled": true
  }
}
```

This configuration will create a harvester with the default sessionizing configuration.

### Sessionizing Configuration Options

The following parameters are optional and will override the defaults specified in the harvester helm `values.yaml`.

#### sessionizing.app

Refers to the scdf app registered with scdf. Needs to be specified if the harvester should not use the default sessionizng app.

#### sessionizing.version

Refers to the app version registered with scdf. Needs to be specified if the harvester should not use the default sessionizing app version.

#### sessionizing.sessionizingAttributeExpression

A SpringEL Expression to extend the grouping of data by a custom attribute \(the processor will always use the correlationId to group data\). With this expression enabled it will use the correlationId **and** the result of this expression. Please make sure that your SpringEL Expression is not causing exceptions, it should include a fallback value in case the attribute is not present in the payload, otherwise the sessionizing will fail.

Example \(without fallback\):

`jsonPath(jsonPath(payload , 'body') , 'data[0].sessionId')`

Example \(with fallback\):

`safeJsonPath(safeJsonPath(payload , 'body') , 'data[0].sessionId')?:'static-fallback-value'`

#### sessionizing.inactivityGapSec

Sessions represent periods of activity separated by gaps of inactivity \(or "idleness"\). An event occurring withing the inactivity gap of an existing session is merged into the existing session. If an event occurs after the session gap, it will start a new session. Value in seconds.

Default:

`1800  # (1800 seconds = 30 minutes)`

If you are processing already sessionized data \(= your payload contains a session id which you have configured \(`sessionizing.sessionizingAttributeExpression`\) make sure your inactivityGap value is not smaller than the inactivity gap used by the source to sessionize the data.

#### sessionizing.gracePeriodSec

Number of seconds to wait until the session is actually closed. This does not extend the inactivity gap, it only enables the correct processing of out-of-order data/late-arrivals. The default value of 120 seconds is a reasonable value for realtime streams. If your harvester is pulling data periodically, this value might be increased depending on the anatomy of your data. Value in seconds. 

Default:

`120   # (120 seconds = 2 minutes)`

#### sessionizing.deploymentConfiguration and sessionizing.appConfiguration

As with other scdf apps certain deployment properties can be provided. Example with all configuration options set for sessionizing:

```text
{
    "displayName": "harvester-1",
    "sourceType": {
        "name": "source-type-1",
        "version": "latest"
    },
    "eventType": {
        "name": "event-type-1",
        "version": "latest"
    },
    "sessionizing": {
        "enabled": true,
        "app": "grnry-sessionizing",
        "version": "latest",
        "correlationIdExpression": "headers['grnry-correlation-id']",
        "sessionizingAttributeExpression": "safeJsonPath(safeJsonPath(payload , 'body') , 'data[0].sessionId')?:'static-fallback-value'",
        "inactivityGapSec": 1800,
        "gracePeriodSec": 120,
        "deploymentConfiguration": {
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.limits.cpu": "500m",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.livenessprobedelay": "120",
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName: 'grnry-pg-citus-secret' , defaultMode: '256' }}]"
        },
        "appConfiguration": {
            "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
            "spring.cloud.stream.bindings.input.consumer.partitioned": "true",
            "spring.cloud.stream.kafka.binder.autoaddpartitions": "true",
            "spring.cloud.stream.kafka.binder.autocreatetopics": "true",
            "spring.cloud.stream.kafka.binder.consumer.maxattempts": "1",
            "spring.cloud.stream.kafka.binder.minpartitioncount": "24",
            "spring.cloud.stream.kafka.binder.replicationfactor": "3"
        }
    }
}
```

## Consuming Sessionized Data

See [Belt Extractor](../../../developer-reference/dataflow/belt-extractor.md) for futher details on how to consume the sessionized data in belts.

