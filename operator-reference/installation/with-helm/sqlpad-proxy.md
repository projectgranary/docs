# SQLPad Proxy

### Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-sqlpad/blob/master/helm-sqlpad-proxy/README.md) for full documentation of all parameters.
{% endhint %}

### Setup

Install Helm Chart:

```text
$ helm install grnry-stable/sqlpad-proxy \
    --name sqlpad-proxy \
    --version <version> \
    -f ./sqlpad-proxy-values.yaml
```

Status check and further instructions:

```text
$ helm status sqlpad-proxy
```

Upgrade Helm Chart:

```text
$ helm upgrade sqlpad-proxy \
    grnry-stable/sqlpad-proxy \
    --version <version> \
    -f ./sqlpad-proxy-values.yaml
```

Tear down Helm Chart: 

```text
helm delete sqlpad-proxy
```

