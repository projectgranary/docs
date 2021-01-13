---
description: >-
  On this page, we describe some tools to make it easier for you to develop with
  Belts. We provide tools such as converters to convert files to single lines.
---

# Converting a script into a one-liner

Using the Belt API, for example for the extractor function, it is necessary to provide a script as a one-liner. As this is very bad to read, it is very likely, you are using a normal editor to create such files. You then want to easily convert these files into one liners.

So, this is our input:

```python
import json
import sys

def lookup(dictionary,keys):
    if type(dictionary)==type(''):
        dictionary=json.loads(dictionary)
    try:
        if len(keys)>1:
            value = lookup(dictionary[keys[0]],keys[1:])
        else:
            value = dictionary[keys[0]]
        return value
    except:
        return None

def execute(event_headers, event_payload, profile = None):
    update = []
    correlation_id= event_headers['grnry-correlation-id']
    geoAltitude=event_payload['geoAltitude']
    payload= lookup(profile,['jsonPayload'])
    opensk = lookup(payload,['openksy'])
    latest = lookup(opensk,['_latest'])
    value = lookup(latest,['_v'])
    update = Update(correlation_id, ['opensky']).set_value(value=str(event_payload['geoAltitude']),_in=int(event_payload['lastContact']), reader='_all')
    update.set_type('opensky')
    return update
```

And we want to convert it to a string, such as this one:

```python
import json\nimport sys\n\ndef lookup(dictionary,keys):\n    if type(dictionary)==type(''):\n        dictionary=json.loads(dictionary)\n    try:\n        if len(keys)>1:\n            value = lookup(dictionary[keys[0]],keys[1:])\n        else:\n            value = dictionary[keys[0]]\n        return value\n    except:\n        return None\n\ndef execute(event_headers, event_payload, profile = None):\n    update = []\n    correlation_id= event_headers['grnry-correlation-id']\n    geoAltitude=event_payload['geoAltitude']\n    payload= lookup(profile,['jsonPayload'])\n    opensk = lookup(payload,['openksy'])\n    latest = lookup(opensk,['_latest'])\n    value = lookup(latest,['_v'])\n    update = Update(correlation_id, ['opensky']).set_value(value=str(event_payload['geoAltitude']),_in=int(event_payload['lastContact']), reader='_all')\n    update.set_type('opensky')\n    return update
```

This string, can be directly entered into our API call to the Belt API. This can be achieved with the following commands.

### Windows

Start `PowerShell` and navigate to the specific resource. Afterwards, you execute the following command:

```bash
get-content <file_name> -Raw | %{$_ -replace "`r`n","\n"} | %{$_ -replace "`"","\\`""}
```

Make sure to replace `<file_name>` with the name of the file you want to convert to string representation. The part ```r`n`` looks for newlines. It might be the case, that this is not working. In such cases, you need either ```r`` or ```n`` separately.

After executing this command, you may copy and paste the value from command line.

### Linux

In Linux, `sed` helps us. First of all, navigate to the resources path. Then execute:

```bash
cat <file_name> | sed -E ':a;N;$!ba;s/\r{0,1}\n/\\n/g' | sed -E 's/"/\\\\"/g'
```

Make sure to replace `<file_name>` with the file you want to replace.

After executing this command, you may copy and paste the value from command line.

### Mac

Mac behaves same as Linux in case we install the package `gnu-sed` and it's command `gsed`. See:

```bash
brew install gnu-sed
cat <file_name> | gsed -E ':a;N;$!ba;s/\r{0,1}\n/\\n/g' | gsed -E 's/"/\\\\"/g'
```

