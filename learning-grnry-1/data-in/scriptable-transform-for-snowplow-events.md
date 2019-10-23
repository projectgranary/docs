---
description: >-
  This page describes how to write a script that transforms snowplow-events into
  corresponding grnry-data-in json-documents and filters them by content.
---

# Scriptable Transform for Snowplow Events

### Transform Thrift to Json‌ <a id="transform-thrift-to-json"></a>

In the preceding section [Getting Started](https://app.gitbook.com/@alvary/s/grnry-sd7f6g8sd68sdf7/~/drafts/-LqL0ROk1Zf3WhF3OJpf/primary/learning-grnry-1/data-in/getting-started#deploying-the-data-in-pipeline) there is a listing containing a call to SCDF. This is where the properties are specified that SCDF will apply to apps in the pipeline. Among others, there is the`app.grnry-scriptable.scriptable-transformer.script` property. Its value is a `String` containing a groovy script that transforms incoming events. For Snowplow events, one important part is the message conversion from `thrift` format to `json` format. The classes needed for deserialization are already contained in the `grnry-scriptable` app. These are `io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe` and `io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload`.

```groovy
import groovy.json.JsonOutput
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload

​SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)​

return JsonOutput.toJson(snowplowEvent)
```

### Handle GET Request Parameters <a id="allow-get-requests"></a>

To transform GET parameters of a snowplow event to a POST-body-like `json` structure, you can use the following script:

```groovy
import groovy.json.JsonSlurper
import groovy.json.JsonOutput
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import java.nio.charset.StandardCharsets

SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)

JsonSlurper jsonSlurper = new JsonSlurper()

// handle GET parameters
if (!snowplowEvent.getBody() && snowplowEvent.getQuerystring()) {
    // create a map ("data") containing all query parameters
    def data = snowplowEvent.getQuerystring()
            .split('&')
            .inject([:]) { map, token ->
                token.split('=').with {
                    def key = URLDecoder.decode(it[0], StandardCharsets.UTF_8.name())
                    def value = URLDecoder.decode(it[1], StandardCharsets.UTF_8.name())
                    if(map.containsKey(key)) {
                        map[key] = ([] + map[key] ?: [map[key]])
                        map[key].add(value)
                    }
                    else map[key] = value
                }
                map
            }
    // build a json structure that matches the correspondyng POST body and assign it to snowplowEvent.body
    snowplowEvent.setBody(JsonOutput.toJson([data:[data]]))
}
return JsonOutput.toJson(snowplowEvent)
```

‌

### Filter Events <a id="filter-events"></a>

Most of the time you don't want all events to pass the same pipeline. So for each pipeline you provide some filter criteria. The following script will search the POST-body of the received snowplow event for the value of `data.[0].filtercriteria` and only forwards it if it matches `"your-filter"`. Otherwise the event is skipped.

```groovy
import groovy.json.JsonSlurper
import groovy.json.JsonOutput
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import java.nio.charset.StandardCharsets

SnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)

JsonSlurper jsonSlurper = new JsonSlurper()

// handle POST body
if (snowplowEvent.getBody()){
    // convert post body to json
    def actualPayload =  jsonSlurper.parseText(snowplowEvent.getBody())
    // check whether it contains a non empty array called "data"
    if (actualPayload?.data && actualPayload?.data[0]) {
        def data = actualPayload.data[0]
        // filter by data.filterCriteria
        if (data?.filterCriteria && data.filterCriteria.equalsIgnoreCase("your-filter")) {
            return JsonOutput.toJson(snowplowEvent)
        }
    }
}

// handle GET parameters
else if (snowplowEvent.getQuerystring()) {
    // create a map ("data") containing all query parameters
    def data = snowplowEvent.getQuerystring()
            .split('&')
            .inject([:]) { map, token ->
                token.split('=').with {
                    def key = URLDecoder.decode(it[0], StandardCharsets.UTF_8.name())
                    def value = URLDecoder.decode(it[1], StandardCharsets.UTF_8.name())
                    if(map.containsKey(key)) {
                        map[key] = ([] + map[key] ?: [map[key]])
                        map[key].add(value)
                    }
                    else map[key] = value
                }
                map
            }
            
    // filter by data.filterCriteria
    if (data?.filterCriteria && data.filterCriteria.equalsIgnoreCase("your-filter")) {
        // build a json structure that matches the correspondyng POST body and assign it to snowplowEvent.body
        snowplowEvent.setBody(JsonOutput.toJson([data:[data]]))
        return JsonOutput.toJson(snowplowEvent)
    }
}

// skip event
return null
```

