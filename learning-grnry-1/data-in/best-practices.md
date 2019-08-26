---
description: 'On this page, we are going to discuss best practices for the usage of SCDF.'
---

# Best Practices

## Naming Conventions

We propose to name the components in the following way:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Component</th>
      <th style="text-align:left">Naming convention</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">registered_apps</td>
      <td style="text-align:left">
        <p>Each application you register.</p>
        <p>For GRNRY apps: grnry_{app_name}</p>
        <p>For your own apps: {your_prefix}_{app_name}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">stream_name</td>
      <td style="text-align:left">
        <p>The name of the stream which you deploy to SCDF.</p>
        <p>harvester_{source_type}_{event_type}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">outbound_topic</td>
      <td style="text-align:left">
        <p>The topic your pipeline writes to:</p>
        <p>grnry_data_in_{event_type}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">event_type</td>
      <td style="text-align:left">
        <p>The event&apos;s name. <b>Please use minus <code>-</code> here</b>.</p>
        <p>{source_application}-{event}</p>
      </td>
    </tr>
  </tbody>
</table>## Scriptable Transform with Python \(Jython\) and Snowplow

The scriptable transform with Python and Snowplow can be used the following way:

At first you need to import the Snowplow Transformers:

```text
from io.grnry.scdfapps.scriptable.snowplow import SnowplowCollectorPayload
from io.grnry.scdfapps.scriptable.snowplow import SnowplowSerDe
```

Then you can access the Snowplow payload as known from Groovy:

```text
snowplowEvent = SnowplowSerDe.deserialize(payload)
```

Afterwards, you may access the content of the snowplowEvent using:

```text
snowplowEvent.getBody()
```

The result of this method contains a JSON String, which needs to be parsed by you.

Once, you are done with your checks, you may convert the whole SnowplowCollectorPayload object to a JSON string using:

```text
SnowplowSerDe.toJSON(snowplowEvent)
```

The result of this is that the whole object is converted to a JSON encoded string. You may pass this back to a variable, such that it is evaluated by SCDF.

## Spring Expression Language \(SpEL\)

The Spring Expression Language is used to define how to pick-up the data from the event in the step [Metadata Extractor](../../developer-reference/dataflow/data-in/metadata-extractor.md).

### Simple JSON Path Evaluations

The message to process in the [Metadata Extractor](../../developer-reference/dataflow/data-in/metadata-extractor.md) contains a header json and a payload json. The Metadata Extractor will search these structures for the provided path and add the found value to the respective new header. The path can be specified using a prefix \(`payload` or `headers`\) followed by the desired path within the json structure. E.g., `payload.some_key.some_other_key`, will return `123` for the following payload:

```javascript
{
    "some_key": {
        "some_other_key": 123
    }
}

```

In addition array access is supported. To get `123` from the payload below, use `payload.some_key[0].some_other_key`: 

```javascript
{  
   "some_key": [  
      {  
         "some_other_key": 123
      },
      "foo",
      "ba",
      {  
         "some_other_key": 4567
      }
   ]
}
```

### Use registered functions

The Metadata Extractor adds the following functions to SpEL which are referenced using `#functionName`:

* `jsonPath(Object json, String jsonPath)` evaluates a `jsonPath` on the provided `object` and returns the `value` object
* `safeJsonPath(Object json, String jsonPath)` evaluates a `jsonPath` on the provided `object` and returns the `value` object or "null" in case the path is not found
* `randomUUID()` returns a random UUID
* `nowMillis()` returns the current time in milliseconds since 1970-01-01 \(UTC\)
* `nowISODateTimeUTC()` returns an ISO formatted String representing the current time \(UTC\)
* `nowISODateUTC()` returns an ISO formatted String representing the current date \(UTC\)
* `dateTimeISOFormatterUTC(String dateTimeString, String pattern)` evaluates the `dateTimeString` and returns a representation of the rsulting DateTime matching the provided `pattern`

So, you want to get the currentMillis for example and hence write:

```javascript
metadata.timestampExpression=#nowMillis()
```

### JsonPath Evaluation of Escaped Strings

To access a value contained within an escaped json structure it is necessary to call the registered function `jsonPath`. As described above, it takes two arguments \(`json`, `jsonPath`\) and will interpret the escaped value found at `jsonPath` in json and return a `json` object that can be accessed via dot-child operator. To get `5678` from the payload below, use the Expression `#jsonPath(payload, 'body').data[0].value` .

```javascript
{
    "body": "{
        \"data\": [
            {
                \"test_id\":\"1234\",
                \"email\":\"value@test.de\",\"harvester\":\"harvester-set-by-path\",
                \"event-type\":\"event-type-set-by-path\",
                \"value\":\"5678\",
                \"path\":[\"path\"]
            }
        ]
    }"
}
```

### Other Operators

#### Ternary Operators

For the below payload `metadata.correlationIdExpression=#someMethod(payload.some_key.some_other_key)` could result in a NullPointerException.

```javascript
{
    "some_key": {
        "some_other_key": null
    }
}
```

#### Elvis Operator

The Elvis operator returns an alternative expression in case the first one evaluates to null.

```javascript
metadata.eventIdExpression=someMethod(payload.some_key.some_other_key?:'not_found')

```

#### Concatenate SpEL

It is possible to concatenate the resulting Strings of SpEL expressions using `'+'`.

* `'name-' + #nowISODateUTC()` results in `"name-2019-06-30T03:10:00.000Z"`
* `payload.payload_id.correlation_id + '-' + #nowISODateUTC()` results in `"123456789-2019-06-30T03:10:00.000Z"`

### Examples 

#### Direct selector:

* from payload \(direct\) : payload.meta.payloadId `metadata.correlationIdExpression=payload.payload_id.correlation_id`
* from payload accessing n-th element of an escaped json array `metadata.eventIdExpression=#jsonPath(payload, 'payload.body').data[0].value`
* from message headers `metadata.correlationIdExpression=headers.timestamp`

#### Use registered functions

* create UUID  `metadata.eventIdExpression=#randomUUID()`
* create current timestamp \(long\)  `metadata.eventIdExpression=#nowMillis()`
* create current timestamp \(ISO Format yyyy-MM-DD'T'....\) `metadata.eventIdExpression=#nowISODateTimeUTC()`
* format timestamp \(ISO Format to custom pattern\) `metadata.correlationIdExpression=#dateTimeISOFormatterUTC('2019-01-01' , 'yyyy') metadata.eventIdExpression=#dateTimeISOFormatterUTC(nowISODateTimeUTC() , 'yyyy-MM')`
* use literal expression \(=static correlationId\)  `metadata.correlationIdExpression='correlation-id-from-properties'`

#### Concat

* lookups\( &lt;payload.meta.knd&gt;-&lt;payload.meta.vertragsdatum&gt; \) `metadata.eventIdExpression=payload.meta.customer + '-' + payload.meta.contractDate`
* lookup + function \( &lt;payload.meta.knd&gt;-TIMESTAMP.NOW\(\)\)  `metadata.eventIdExpression=payload.meta.customer + '-' + #nowISODateUTC()`

#### Other

* ternaries `metadata.eventIdExpression=payload.meta.payloadId?:payload.meta.alternativePayloadId`
* ternaries with operators :
  * if "payload.meta.payloadId exists and does not start with "ID-" use payload.meta.payloadId otherwise use payload.meta.alternativePayloadId"`metadata.eventIdExpression=payload.meta.payloadId.startsWith('ID-') ? payload.meta.payloadId : payload.meta.alternativePayloadId`
* Elvis Operator `"payload.meta.payloadId?:'NOTSET'"`

