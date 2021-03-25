---
description: >-
  On this page, we describe some tools to make it easier for you to develop with
  Harvester API. We provide tools such as converters to convert files to single
  lines.
---

# Converting a script into a one-liner

Your groovy/gradle script has to be transformed into a one-liner and added to the harvester definition. As this is very bad to read, it is very likely, you are using a normal editor to create such files. You then want to easily convert these files into one liners.

So, this is our input:

```groovy
import groovy.json.JsonSlurper
import groovy.json.JsonOutput
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import java.util.logging.Logger
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload

Logger logger = Logger.getLogger("groovy-transform_test")
JsonSlurper jsonSlurper = new JsonSlurper()
try{
    logger.info("payload: " + payload)
    def jsonPayload =  jsonSlurper.parseText(new String(payload, 'UTF-8'))
    def actualPayload =  jsonSlurper.parseText(jsonPayload.body)
    logger.info("actualPayload " + actualPayload)
    def data = actualPayload.data[0]
    logger.info("data: " + data)
        logger.info("path: " + path)
        if (data."$path") {
            logger.info("value: " + (String) data."$path")
            return JsonOutput.toJson(jsonPayload)
        }

}
catch (Exception e){
    e.printStackTrace()
    return null
}

return null
```

And we want to convert it to a string, such as this one:

```groovy
import groovy.json.JsonSlurper\\nimport groovy.json.JsonOutput\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe\\nimport java.util.logging.Logger\\nimport io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload\\n\\nLogger logger = Logger.getLogger("groovy-transform_test")\\nJsonSlurper jsonSlurper = new JsonSlurper()\\ntry{\\n    logger.info("payload: " + payload)\\n    def jsonPayload =  jsonSlurper.parseText(new String(payload, 'UTF-8'))\\n    def actualPayload =  jsonSlurper.parseText(jsonPayload.body)\\n    logger.info("actualPayload " + actualPayload)\\n    def data = actualPayload.data[0]\\n    logger.info("data: " + data)\\n        logger.info("path: " + path)\\n        if (data."$path") {\\n            logger.info("value: " + (String) data."$path")\\n            return JsonOutput.toJson(jsonPayload)\\n        }\\n\\n}\\ncatch (Exception e){\\n    e.printStackTrace()\\n    return null\\n}\\n\\nreturn null\\n
```

This string, can be directly entered into our call to the Harvester API. This can be achieved with the following command:

### Windows

Start `PowerShell` and navigate to the specific resource. Afterwards, you execute the following command:

```bash
get-content <file_name> -Raw | %{$_ -replace "`r`n","\n"} | %{$_ -replace "`"","\\`""}
```

Make sure to replace `<file_name>` with the name of the file you want to convert to string representation. The part ```r`n`` looks for newlines. It might be the case, that this is not working. In such cases, you need either ```r`` or ```n`` separately.

After executing this command, you may copy and paste the value from command line.

### Linux

In order to achieve this with Linux, `sed` helps us. First of all, navigate to the resources path. Then execute:

```bash
cat <file_name> | sed -E ':a;N;$!ba;s/\r{0,1}\n/\\n/g' | sed -E 's/"/\\\\"/g'
```

### Mac

Mac behaves same as Linux in case we install the package `gnu-sed` and it's command `gsed`. See:

```bash
brew install gnu-sed
cat <file_name> | gsed -E ':a;N;$!ba;s/\r{0,1}\n/\\n/g' | gsed -E 's/"/\\\\"/g'
```

