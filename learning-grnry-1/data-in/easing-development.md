---
description: >-
  On this page, we describe some tools to make it easier for you to develop with
  SCDF. We provide tools such as converters to convert files to single lines.
---

# Easing development

## Converting a script into a one-liner

Using SCDF, for example for the scriptable transform, it is necessary to provide a Script as a one-liner. As this is very bad to read, it is very likely, you are using a normal editor to create such files. You then want to easily convert these files into one liners.

So, this is our example input:

```groovy
import groovy.json.JsonSlurperimport groovy.json.JsonOutputimport io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDeimport io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayloadimport java.nio.charset.StandardCharsetsSnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)JsonSlurper jsonSlurper = new JsonSlurper()if (snowplowEvent.getBody()){    def actualPayload =  jsonSlurper.parseText(snowplowEvent.getBody())    if (actualPayload?.data && actualPayload?.data[0]) {        def data = actualPayload.data[0]        if (data?.filterCriteria && data.filterCriteria.equalsIgnoreCase("a")) {            return JsonOutput.toJson(snowplowEvent)        }    }}else if (snowplowEvent.getQuerystring()) {    def data = snowplowEvent.getQuerystring()            .split('&')            .inject([:]) { map, token ->                token.split('=').with {                    def key = URLDecoder.decode(it[0], StandardCharsets.UTF_8.name())                    def value = URLDecoder.decode(it[1], StandardCharsets.UTF_8.name())                    if(map.containsKey(key)) {                        map[key] = ([] + map[key] ?: [map[key]])                        map[key].add(value)                    }                    else map[key] = value                }                map            }    if (data?.filterCriteria && data.filterCriteria.equalsIgnoreCase("a")) {        snowplowEvent.setBody(JsonOutput.toJson([data:[data]]))        return JsonOutput.toJson(snowplowEvent)    }}return null
```

And we want to convert it to a string, such as this one:

```groovy
import groovy.json.JsonSlurper\\r\\nimport groovy.json.JsonOutput\\r\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe\\r\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload\\r\\n\\r\\nimport java.nio.charset.StandardCharsets\\r\\n\\r\\nSnowplowCollectorPayload snowplowEvent = SnowplowSerDe.deserialize(payload)\\r\\n\\r\\nJsonSlurper jsonSlurper = new JsonSlurper()\\r\\n\\r\\nif (snowplowEvent.getBody()){\\r\\n    def actualPayload =  jsonSlurper.parseText(snowplowEvent.getBody())\\r\\n    if (actualPayload?.data && actualPayload?.data[0]) {\\r\\n        def data = actualPayload.data[0]\\r\\n        if (data?.filterCriteria && data.filterCriteria.equalsIgnoreCase(\\\"a\\\")) {\\r\\n            return JsonOutput.toJson(snowplowEvent)\\r\\n        }\\r\\n    }\\r\\n}\\r\\nelse if (snowplowEvent.getQuerystring()) {\\r\\n    def data = snowplowEvent.getQuerystring()\\r\\n            .split('&')\\r\\n            .inject([:]) { map, token ->\\r\\n                token.split('=').with {\\r\\n                    def key = URLDecoder.decode(it[0], StandardCharsets.UTF_8.name())\\r\\n                    def value = URLDecoder.decode(it[1], StandardCharsets.UTF_8.name())\\r\\n                    if(map.containsKey(key)) {\\r\\n                        map[key] = ([] + map[key] ?: [map[key]])\\r\\n                        map[key].add(value)\\r\\n                    }\\r\\n                    else map[key] = value\\r\\n                }\\r\\n                map\\r\\n            }\\r\\n    if (data?.filterCriteria && data.filterCriteria.equalsIgnoreCase(\\\"a\\\")) {\\r\\n        snowplowEvent.setBody(JsonOutput.toJson([data:[data]]))\\r\\n        return JsonOutput.toJson(snowplowEvent)\\r\\n    }\\r\\n}\\r\\nreturn null
```

### Windows

In order to achieve this with windows, start PowerShell and navigate to the specific resource. Afterwards, you may execute the following command.

```bash
get-content <file_name> -Raw | %{$_ -replace "`r`n","\\n"}
```

Make sure to replace `<file_name>` with the name of the file you want to convert to string representation. The part \`r\`n looks for newlines. It might be the case, that this is not working. In such cases, you need either ```r`` or ```n`` separately.

After executing this command, you may copy and paste the value from command line.

### Linux

In order to achieve this with Linux, `sed` helps us. First of all, navigate to the resources path. Then execute:

```text
cat <file_name> | sed -E ':a;N;$!ba;s/\r{0,1}\n/\\\\n/g'
```

Make sure to replace `<file_name>` with the file you want to replace.

After executing this command, you may copy and paste the value from command line.

## Converting SCDF UI Properties to a command line syntax

The SCDF Dashboard \(the graphical user-interface\) allows us to copy the current deployment parameters. You might want to use these to query a command from the command line.

So, how do you get the raw data.

Navigate to the streams area in your SCDF Dashboard:

![](../../.gitbook/assets/grafik%20%289%29.png)

Now, open one stream:

![](../../.gitbook/assets/grafik%20%288%29.png)

Next, click on `update stream`:

![](../../.gitbook/assets/grafik%20%2810%29.png)

Now, click on `Copy to Clipboard`:

![](../../.gitbook/assets/grafik%20%287%29.png)

{% hint style="danger" %}
The copy to clipboard does not correctly copy your script for scriptable-transform. Therefore, you need to enter it manually using the method specified above. You should add it after executing the script below.
{% endhint %}

### Windows

In Windows you can execute the following command in Power-Shell to make it available for your JSON section within the deploy stream cURL:

```bash
Echo <copied_text> |\ %{$_ -replace "`n", """"", """""} |\ %{$_ -replace "=", """"":"""""} |\ %{Write-Host '""'$_'""' }
```

Make sure to replace `<copied_text>` with the text from clipboard. In line 2, we match every new line and replace it with `"", ""`. In line 3, we replace the `=` with `"":""` and in line 4 we define a preceeding `""` and a succeeding `""`.

### Linux

In Linux you can execute the following command in a console, to make the application properties available as JSON String.

```bash
echo "<copied_text>" |\ sed -E ':a;N;$!ba;s/\r{0,1}\n/"", ""/g' |\ sed -E 's/=/"":""/g' |\ (echo -n '""' && cat) |\ sed 's/$/""/'
```

Make sure to replace `<copied_text>` with the text from clipboard. In line 2 we replace new lines with `"", ""`. In line 3, we replace the `=` with `"":""`. In line 4 we prepend `""` and in line 5 we append `""`.

