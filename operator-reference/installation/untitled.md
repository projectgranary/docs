# Belt Extractor

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/belt-extractor/tree/master/helm](https://gitlab.alvary.io/grnry/belt-extractor/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/grnry-belt-openshift \
    --name grnry-belt-<belt-name> \
    --version <version> \
    -f ./grnry-belt-<belt-name>-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-belt-<belt-name>
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-belt-<belt-name> \
    grnry-stable/grnry-belt-<belt-name> \
    --version <version> \
    -f ./grnry-belt-<belt-name>-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-belt-<belt-name>
```

