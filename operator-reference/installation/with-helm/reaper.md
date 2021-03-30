# Reaper

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-reaper/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-stable/reaper \
    --name reaper \
    --version <version> \
    -f ./reaper-values.yaml
```

Status check and further instructions:

```text
$ helm status reaper
```

Upgrade Helm Chart:

```text
$ helm upgrade reaper \
    grnry-stable/reaper \
    --version <version> \
    -f ./reaper-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete reaper
```

## Index on Profile Store for Reaper

{% hint style="info" %}
Starting from Granary 0.9.1, the Reaper requires the following index to be present on the Profile Store.
{% endhint %}

#### PostgreSQL Function

```text
CREATE or REPLACE FUNCTION profile_time_to_act(inserted bigint, ttl varchar) RETURNS bigint AS $$

SELECT inserted/1000 + cast(extract(epoch from ttl::interval) as bigint);

$$ LANGUAGE SQL IMMUTABLE;
```

#### Index

```text
CREATE INDEX IF NOT EXISTS profilestore_time_to_act ON public.profilestore (profile_time_to_act(inserted, ttl));
CREATE INDEX IF NOT EXISTS profilestore_time_to_notify ON public.profilestore (profile_time_to_act(inserted, ttn));
```

