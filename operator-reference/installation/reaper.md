# Reaper

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/reaper/-/tree/master/helm](https://gitlab.alvary.io/grnry/reaper/-/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
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
$ helm delete --purge reaper
```

