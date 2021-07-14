---
description: Migration guide to upgrade to Belt Extractor 1.1
---

# Belt Extractor 1.1

With the Release of Granary 1.1 Cliff the semantics of the Belt extractor's `Update` Object received a refactoring of the `set_value` parameter order that need to be considered.

The grain value of the `Update` object used to contain a `_ttl` holding a time to live. After the time expired there was a notification \(no deletion\) event emitted by the Granary Reaper.

#### Grain value with 1.0 Aretha

```yaml
GRAIN_VALUE :=
  {
    "_v": string,                               # Json object string, Json array string, or string of counter value. Value type must fit _operation
    "_c": double,                               # default is 1
    "_in": long,                                # default is now()
    "_ttl": string,                             # period of time, https://en.wikipedia.org/wiki/ISO_8601#Durations, default is "P100Y"
    "_origin": string,                          # default is "/belts/{belt-id}"
    "_reader": string                           # default is "_all"
  }
```

In Granary 1.1 Cliff the grain value was complemented by a `_ttn` field. This, explicitly, holds a 'time till notification' whereas the `_ttl` should, from now on, hold an actual 'time to live' after which it will be removed automatically.

#### Grain value with 1.1 Cliff

```yaml
GRAIN_VALUE :=
  {
    "_v": string,                               # Json object string, Json array string, or string of counter value. Value type must fit _operation
    "_c": double,                               # default is 1
    "_in": long,                                # default is now()
    "_ttl": string,                             # period of time, https://en.wikipedia.org/wiki/ISO_8601#Durations, default is "P100Y"
    "_ttn": string,                             # period of time, https://en.wikipedia.org/wiki/ISO_8601#Durations, default is "P100Y"
    "_origin": string,                          # default is "/belts/{belt-id}"
    "_reader": string                           # default is "_all"
  }
```

To preserve old belts with a short `_ttl` that were presumably written considering a 'time till notification' semantic, we changed the parameter list ordering of the `Update.set_value()` method as follows:

#### Old set\_value method definition

```python
def set_value(
    self, 
    value, 
    certainty=1.0, 
    _in=time(), 
    ttl='P100Y',
    origin=DEFAULT_BELT_ORIGIN, 
    reader='_auth')
```

#### New set\_value method definition

```python
def set_value(
    self, 
    value, 
    certainty=1.0, 
    _in=time(), 
    ttn='P100Y',
    origin=DEFAULT_BELT_ORIGIN, 
    reader='_auth',
    ttl='P100Y')
```

So, the call below from a Granary 1.1 Cliff belt would make `'P1D'` a **TTN** not **TTL** as it would have done in Granary 1.0 Aretha.

#### Example:

```python
update.set_value('Hallo Belt!',0.5,time(),'P1D','Dummy-Belt')
```

**New \_set\_ttn and \_set\_ttl operations**

We also introduced the new `_set_ttn` and `_set_ttl` operation which allow you to manipulate solely a grains TTN or TTL respectively:

```python
Update(correlation_id, path, operation='_set_ttl').set_value(value='P5M')
```

