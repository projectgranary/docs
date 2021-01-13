# Event Store API

## Chart Home

{% hint style="info" %}
See [Helm Chart's README.md](https://github.com/syncier/grnry-eventstore-api/tree/master/helm) for full documentation of all parameters.
{% endhint %}

## Setup

Install Helm Chart:

```text
$ helm install grnry-stable/eventstore-api \
    --name grnry-eventstore-api \
    --version <version> \
    -f ./grnry-eventstore-api-values.yaml
```

Status check and further instructions:

```text
$ helm status grnry-eventstore-api
```

Upgrade Helm Chart:

```text
$ helm upgrade grnry-eventstore-api \
    grnry-stable/eventstore-api \
    --version <version> \
    -f ./grnry-eventstore-api-values.yaml
```

Tear down Helm Chart:

```text
$ helm delete grnry-eventstore-api
```

## Configuration

{% tabs %}
{% tab title="Spec" %}
| Environment Variable | Value | Description |
| :--- | :--- | :--- |
| JAVA\_OPTS | Xms512m -Xmx512m | JVM specific configuration like head-size, GC settings, ... |
| SPRING\_PROFILE | PROD | Currently not used, provide a dummy value like 'PROD' |
| EVENTSTORE\_DB\_HOSTNAME | grnry-pg | Hostname of the Postgres instance |
| EVENTSTORE\_DB\_PORT | 5432 | Port of the Postgres instance |
| EVENTSTORE\_DB\_DATABASE | postgres | Database of the Postgres instance |
| EVENSTORE\_DB\_USERNAME | username | Database account username |
| EVENSTORE\_DB\_PASSWORD | password | Database account password |
| EVENTSTORE\_DB\_SSL | false | Whether database connection should use SSL |
| EVENTSTORE\_DB\_SSL\_VALIDATE | false | Whether server certificate should be validated at secure database connection |
{% endtab %}
{% endtabs %}

