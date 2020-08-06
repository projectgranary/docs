# Profile Store API

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-profilestore-api/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```
$ helm install grnry-stable/profilestore-api \
    --name grnry-profilestore-api \
    --version <version> \
    -f ./grnry-profilestore-api-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-profilestore-api
```

Upgrade Helm Chart: 

```text
$ helm upgrade grnry-profilestore-api \
    grnry-stable/profilestore-api \
    --version <version> \
    -f ./grnry-profilestore-api-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-profilestore-api
```

## Configuration

{% tabs %}
{% tab title="Spec" %}
| Environment Variable | Value | Description |
| :--- | :--- | :--- |
| JAVA\_OPTS | Xms512m -Xmx512m | JVM specific configuration like head-size, GC settings, ... |
| SPRING\_PROFILE | PROD | Currently not used, provide a dummy value like 'PROD' |
| PROFILESTORE\_DB\_HOSTNAME | grnry-pg-citus-0.grnry-pg-citus | Hostname of the Postgres instance |
| PROFILESTORE\_DB\_PORT | 5432 | Port of the Postgres instance |
| PROFILESTORE\_DB\_DATABASE | postgres | Database of the Postgres instance |
| PROFILESTORE\_DB\_USERNAME | username | Database account username |
| PROFILESTORE\_DB\_PASSWORD | password | Database account password |
| PROFILESTORE\_DB\_SSL | false | Whether database connection should use SSL |
| PROFILESTORE\_DB\_SSL\_VALIDATE | false | Whether server certificate should be validated at secure database connection |
{% endtab %}
{% endtabs %}

