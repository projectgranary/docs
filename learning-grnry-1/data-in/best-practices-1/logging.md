---
description: >-
  On this page we explain how to use logging in the scriptable transform for
  easier monitoring.
---

# Logging

All transformation scripts utilise the GRNRY logger. The GRNRY logger will output all its messages with the common log format as specified [here](../../../operator-reference/site-reliability/common-log-format.md). Any logs you create by importing the `java.util.logging.Logger` inside the script will appear in the message part of the common GRNRY log format. Below you can find an example what logging in a groovy script could look like:

```groovy
import groovy.json.JsonBuilder
import groovy.json.JsonSlurper
import groovy.json.JsonOutput
import io.grnry.scdfapps.scriptable.snowplow.SnowplowSerDe
import io.grnry.scdfapps.scriptable.snowplow.SnowplowCollectorPayload
import java.util.logging.Logger


Logger logger = Logger.getLogger('testLogger')
logger.info('An example log.')
```

{% hint style="danger" %}
Please note that if you write messages to STDOUT \(e.g. via the print\(\)-method\), your messages won't be in the common GRNRY logformat and thus will not contain additional information beside the message itself.
{% endhint %}

