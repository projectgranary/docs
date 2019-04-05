# Metadata Extractor

## Chart Home

{% hint style="info" %}
See for full documentation of all parameters:  
[https://gitlab.alvary.io/grnry/grnry-extractor/tree/master/helm](https://gitlab.alvary.io/grnry/grnry-extractor/tree/master/helm)
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/grnry-extractor \
    --name grnry-metadata-extractor \
    --version <version> \
    -f ./grnry-metadata-extractor-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-metadata-extractor 
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-metadata-extractor \
    grnry-stable/grnry-metadata-extractor \
    --version <version> \
    -f ./grnry-metadata-extractor-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete --purge grnry-metadata-extractor
```

