# Sessionizing Processor

The Sessionizing processor is used to group data based on inactivity. You can read more about the algorithm here: [https://kafka.apache.org/20/documentation/streams/developer-guide/dsl-api.html#session-windows](https://kafka.apache.org/20/documentation/streams/developer-guide/dsl-api.html#session-windows).

### Required headers:

* grnry-correlation-id
* grnry-event-timestamp

### Configuration options

**Name**: grnry-sessionizing-processor

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

### Sessionizing Configuration Options

#### sessionizing.sessionizingAttributeExpression

A SpringEL Expression to extend the grouping of data by a custom attribute (the processor will always use the correlationId to group data). With this expression enabled it will use the correlationId **and** the result of this expression. Please make sure that your SpringEL Expression is not causing exceptions, it should include a fallback value in case the attribute is not present in the payload, otherwise the sessionizing will fail.

Example (without fallback):

`jsonPath(jsonPath(payload , 'body') , 'data[0].sessionId')`

Example (with fallback):

`safeJsonPath(safeJsonPath(payload , 'body') , 'data[0].sessionId')?:'static-fallback-value'`

#### sessionizing.inactivityGap

Sessions represent periods of activity separated by gaps of inactivity (or "idleness"). An event occurring withing the inactivity gap of an existing session is merged into the existing session. If an event occurs after the session gap, it will start a new session. Value in seconds.

Default:

`1800  # (1800 seconds = 30 minutes)`

If you are processing already sessionized data (i.e. your payload contains a session id, which you have configured via`sessionizing.sessionizingAttributeExpression`) make sure your `sessionizing.inactivityGap` value reflects the sessionizing of your source data, e.g., is not smaller than the gap used by the source.

#### sessionizing.gracePeriod

Number of seconds to wait until the session is actually closed. This does not extend the inactivity gap, it only enables the correct processing of out-of-order data/late-arrivals. The default value of 120 seconds is a reasonable value for realtime streams. If your harvester is pulling data periodically, this value might be increased depending on the anatomy of your data. Value in seconds.&#x20;

Default:

`120   # (120 seconds = 2 minutes)`

#### &#x20;sessionizing.eventTypeName

&#x20;The event type name.

#### sessionizing.eventTypeVersion

The event type version.

Default:

`1`

#### sessionizing.harvesterName

&#x20;The harvester name.



### Full Example

Additionally, you need to fill the following parameters with [Spring Expression Language](../../../learning-grnry-1/data-in/best-practices-1/best-practices.md) expressions:

```yaml
sessionizing.correlationIdExpression : [SpEL-Expression - optional]
sessionizing.sessionizingAttributeExpression: [SpEL-Expression - optional]
sessionizing.inactivityGap : [long literal in seconds]
sessionizing.gracePeriod : [long literal in seconds]
sessionizing.eventTypeName: [String literal]
sessionizing.eventTypeVersion: [String literal]
sessionizing.harvesterName : [String literal]
```

Example:

```yaml
"sessionizing": {
    "app": "grnry-sessionizing",
    "version": "latest",
    "deploymentConfiguration": {
        "kubernetes.imagepullpolicy": "Always",
        "kubernetes.limits.cpu": "500m",
        "kubernetes.limits.memory": "512Mi",
        "kubernetes.livenessprobedelay": "120",
        "kubernetes.readinessprobedelay": "120",
        "kubernetes.requests.cpu": "500m",
        "kubernetes.requests.memory": "512Mi",
        "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_private.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
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
    },
    "correlationIdExpression": "headers['grnry-correlation-id']",
    "sessionizingAttributeExpression": "payload.correlationId",
    "inactivityGapSec": 1800,
    "gracePeriodSec": 120,
    "enabled": true
}
```



