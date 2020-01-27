# Sessionize Data

## About Sessionizing <a id="about-sessionizing"></a>

Taken from [https://kafka.apache.org/20/documentation/streams/developer-guide/dsl-api.html\#session-windows](https://kafka.apache.org/20/documentation/streams/developer-guide/dsl-api.html#session-windows):

Session windows are used to aggregate key-based events into so-called _sessions_, the process of which is referred to as _sessionization_. Sessions represent a **period of activity** separated by a defined **gap of inactivity** \(or “idleness”\). Any events processed that fall within the inactivity gap of any existing sessions are merged into the existing sessions. If an event falls outside of the session gap, then a new session will be created.

![](../../.gitbook/assets/image%20%2811%29.png)

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

## Add Sessionizing to a harvester

This section describes how to include the sessionizing processor in manual creation of a harvester. As soon as the [Harvester API](../../developer-reference/api-reference/harvester-api.md) is fully implemented, harvester creation will be completely rest-driven and include sessionizing configuration.

### Registering the SCDF app

{% hint style="info" %}
This section is an addition to [Getting Started](getting-started.md), it will not explain in detail how to create a harvester, but only how to register the necessary application and how to add the additional processing step to a standard \(non-sessionizing\) harvester. 
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

### Extending the harvester definition

A \(non-sessionizing\) harvester definition looks like this:

```text
jdbc-source: grnry-jdbc-source | script: grnry-scriptable-processor | meta: grnry-data-in-metadata-processor > :grnry_data_in_target
```

Add a `| session: grnry-sessionizing-processor` between the grnry-data-in-metadata-processor and the topic sink:

```text
jdbc-source: grnry-jdbc-source | script: grnry-scriptable-processor | meta: grnry-data-in-metadata-processor | session: grnry-sessionizing-processor > :grnry_data_in_target
```

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

#### sessionizing.eventTypeName

The event type name.

#### sessionizing.harvestName

The harvester name.

### Example Harvester Configuration

In the enhanced harvester definition above we use the alias "session" for the grnry-sessionizing-processor, therefore we need to prefix the configuration options with app.session.

Example with all configuration options set:

```text
curl <URL to SCDF>/streams/deployments/<stream_name> -i -X POST \
 -H "Content-Type: application/json" -u <user>:<password> \
 -d "{\
  ""app.session.sessionizing.sessionizingAttributeExpression"":""safeJsonPath(safeJsonPath(payload , 'body') , 'data[0].sessionId')?:'static-fallback-value'"", \
  ""app.session.sessionizing.inactivityGap"" : 3600 , \
  ""app.session.sessionizing.gracePeriod"" : 120 , \
  ""app.session.sessionizing.eventTypeName"": ""${grnry.eventTypeName}"",\
  ""app.session.sessionizing.harvesterName"": ""${grnry.harvesterName}"",\
  ""app.session.grnry.harvesterName"" : ""<harvester_name>"" ,\
  ""app.session.grnry.eventTypeName"" : ""<event_name>"" ,\
 ... // rest of your harvester definition (sourcetype / scritable-transform / metadata extractor)
```

## Consuming Sessionized Data

See [Belt Extractor](../../developer-reference/dataflow/belt-extractor.md) for futher details on how to consume the sessionized data in belts.

