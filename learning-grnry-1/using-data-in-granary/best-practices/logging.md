---
description: On this page we explain how to use logging in a belt for easier monitoring.
---

# Logging

All belts ship with the GRNRY logger preconfigured. The GRNRY logger will output all its messages with the common log format as specified [here](../../../operator-reference/site-reliability/common-log-format.md). To make use of the GRNRY logger with its additional information such as Zipkin Trace IDs you need to use it as such:

```python
from grnry.beltextractor.models.update import Update
import logging


def execute(event_headers, event, profile=None):
    
    logging.info("Event headers follow:")
    logging.info(event_headers)
    
    ...
    return updates
```

{% hint style="danger" %}
Please note that if you write messages to STDOUT \(e.g. via the print\(\)-method\), your messages won't be in the common GRNRY logformat and thus will not contain additional information beside the message itself.
{% endhint %}

