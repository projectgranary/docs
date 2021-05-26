# Eventstore Reaper

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-event-reaper/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-stable/reaper \
    --name event-reaper \
    --version <version> \
    -f ./event-reaper-values.yaml
```

Status check and further instructions:

```text
$ helm status event-reaper
```

Upgrade Helm Chart:

```text
$ helm upgrade event-reaper \
    grnry-stable/event-reaper \
    --version <version> \
    -f ./event-reaper-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete event-reaper
```

## Index on Event Store for Reaper

#### Index

```sql
 CREATE INDEX IF NOT EXISTS eventstore_time_to_act ON public.eventstore (event_time_to_act(created, ttl));
```

