# SQLPad

### Chart Home

{% hint style="info" %}
See [Helm Chart's Readme.md](https://github.com/syncier/grnry-sqlpad/blob/master/helm-sqlpad-instance/README.md) for full documentation of all parameters
{% endhint %}

### Setup

{% hint style="warning" %}
To function correctly, this chart requires a deployed [SQLPad Proxy](sqlpad-proxy.md) chart.
{% endhint %}

Install Helm Chart

```text
$ helm install grnry-stable/sqlpad-instance \
    --name sqlpad-instance \
    --version <version> \
    -f ./sqlpad-instance-values.yaml
```

Status check and further instructions:

```text
$ helm status sqlpad-instance
```

Upgrade Helm Chart:

```text
$ helm upgrade sqlpad-instance \
    grnry-stable/sqlpad-instance \
    --version <version> \
    -f ./sqlpad-instance-values.yaml
```

Tear down Helm Chart:

```text
helm delete sqlpad-instance
```

