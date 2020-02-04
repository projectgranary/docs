# Sessionizing Processor

The Sessionizing processor is used to group data based on inactivity. You can read more about the algorithm here: [https://kafka.apache.org/20/documentation/streams/developer-guide/dsl-api.html\#session-windows](https://kafka.apache.org/20/documentation/streams/developer-guide/dsl-api.html#session-windows).

### Required headers:

* grnry-correlation-id
* grnry-time-stamp

### Configuration options

**Name**: grnry-sessionizing-processor

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

### Sessionizing Configuration Options

#### sessionizing.sessionizingAttributeExpression

A SpringEL Expression to extend the grouping of data by a custom attribute \(the processor will always use the correlationId to group data\). With this expression enabled it will use the correlationId **and** the result of this expression. Please make sure that your SpringEL Expression is not causing exceptions, it should include a fallback value in case the attribute is not present in the payload, otherwise the sessionizing will fail.

Example \(without fallback\):

`jsonPath(jsonPath(payload , 'body') , 'data[0].sessionId')`

Example \(with fallback\):

`safeJsonPath(safeJsonPath(payload , 'body') , 'data[0].sessionId')?:'static-fallback-value'`

#### sessionizing.inactivityGap

Sessions represent periods of activity separated by gaps of inactivity \(or "idleness"\). An event occurring withing the inactivity gap of an existing session is merged into the existing session. If an event occurs after the session gap, it will start a new session. Value in seconds.

Default:

`1800  # (1800 seconds = 30 minutes)`

If you are processing already sessionized data \(= your payload contains a session id which you have configured \(`sessionizing.sessionizingAttributeExpression`\) make sure your inactivityGap value is not smaller than the inactivity gap used by the source to sessionize the data.

#### sessionizing.gracePeriod

Number of seconds to wait until the session is actually closed. This does not extend the inactivity gap, it only enables the correct processing of out-of-order data/late-arrivals. The default value of 120 seconds is a reasonable value for realtime streams. If your harvester is pulling data periodically, this value might be increased depending on the anatomy of your data. Value in seconds. 

Default:

`120   # (120 seconds = 2 minutes)`

####  sessionizing.eventTypeName

 The event type name.

#### sessionizing.harvestName

 The harvester name.



### Full Example

Additionally, you need to fill the following parameters with Spring Expression Language expressions:

```yaml
sessionizing.correlationIdExpression : [SpEL-Expression - optional]
sessionizing.sessionizingAttributeExpression: [SpEL-Expression - optional]
sessionizing.inactivityGap : [long literal in seconds]
sessionizing.gracePeriod : [long literal in seconds]
sessionizing.eventTypeName: [String literal]
sessionizing.harvesterName : [String literal]
```

Example:

```yaml
app.grnry-sessionizing-processor.sessionizing.correlationIdExpression: "headers['grnry-correlation-id']",
app.grnry-sessionizing-processor.sessionizing.sessionizingAttributeExpression: "payload.correlationId",
app.grnry-sessionizing-processor.sessionizing.eventTypeName: "${grnry.eventTypeName}",
app.grnry-sessionizing-processor.sessionizing.harvesterName: "${grnry.harvesterName}",
app.grnry-sessionizing-processor.sessionizing.inactivityGap: 3600,
app.grnry-sessionizing-processor.sessionizing.gracePeriod : 120,
app.grnry-sessionizing-processor.grnry.harvesterName: "sftp-all-std-harvester",
app.grnry-sessionizing-processor.grnry.eventTypeName: "sftp-all" 
```

In the example the part `grnry-sessionizing-processor` \(the app name\) has to be replaced by an alias, if one has been specified in the stream definition.

{% hint style="info" %}
Due to technical limitations in the [Event Store API](../../api-reference/event-store-api.md), the `eventTypeName`and `harvesterName` may not contain underscores "`_`".
{% endhint %}

**Source Parameters:**

The current documentation for the Spring Expression Language \(SpEL\) can be found here:

[https://docs.spring.io/spring/docs/5.1.8.RELEASE/spring-framework-reference/core.html\#expressions-language-ref](https://docs.spring.io/spring/docs/5.1.8.RELEASE/spring-framework-reference/core.html#expressions-language-ref)

