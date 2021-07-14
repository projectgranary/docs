# Grafana Dashboards

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-grafana-dashboards/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-stable/grnry-grafana-dashboards \
    --name grnry-grafana-dashboards \
    --version <version> \
    -f ./grnry-grafana-dashboards-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-grafana-dashboards
```

Upgrade Helm Chart:

```text
$ helm upgrade grnry-grafana-dashboards \
    grnry-stable/grnry-grafana-dashboards \
    --version <version> \
    -f ./grnry-grafana-dashboards-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-grafana-dashboards
```

