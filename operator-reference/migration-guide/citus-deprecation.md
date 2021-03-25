---
description: Changed default database configurations
---

# Citus Deprecation

To configure your database in your Granary instance, you should have an eye on the default database configurations \(in values.yaml\) that changed with Granary 1.0 in course of deprecating Citus. 

### Harvester Api 

<table>
  <thead>
    <tr>
      <th style="text-align:left">Spec</th>
      <th style="text-align:left">Previous default</th>
      <th style="text-align:left">Current default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>postgres.host</code>
      </td>
      <td style="text-align:left"><code>grnry-pg-citus</code>
      </td>
      <td style="text-align:left"><code>grnry-pg</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>postgres.secretName</code>
      </td>
      <td style="text-align:left"><code>grnry-pg-citus-secret</code>
      </td>
      <td style="text-align:left"><code>grnry-pg-credentials</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>postgres.secretKeyUsername</code>
      </td>
      <td style="text-align:left"><code>superuser-username</code>
      </td>
      <td style="text-align:left"><code>postgresql-username</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>postgres.secretKeyPassword</code>
      </td>
      <td style="text-align:left"><code>superuser-password</code>
      </td>
      <td style="text-align:left"><code>postgresql-password</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>eventstores.pg.persister.</code>
        </p>
        <p><code>deploymentConfig.kubernetes.volumes</code>
        </p>
      </td>
      <td style="text-align:left"><code>&quot;[{name: &apos;secret&apos;, secret: { secretName : &apos;grnry-base-encryption-token&apos; , defaultMode : &apos;256&apos; }}, {name: &apos;db-secret&apos;, secret: { secretName : &apos;grnry-pg-citus-secret&apos; , defaultMode : &apos;256&apos; }}]&quot;</code>
      </td>
      <td style="text-align:left"><code>&quot;[{name: &apos;secret&apos;, secret: { secretName : &apos;grnry-base-encryption-token&apos; , defaultMode : &apos;256&apos; }}, {name: &apos;db-secret&apos;, secret: { secretName : &apos;grnry-pg-credentials&apos; , defaultMode : &apos;256&apos; }}]&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>eventstores.pg.persister.</code>
        </p>
        <p><code>appConfig.spring.datasource.password</code>
        </p>
      </td>
      <td style="text-align:left"><code>${superuser-password}</code>
      </td>
      <td style="text-align:left"><code>${postgresql-password}</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>eventstores.pg.persister.</code>
        </p>
        <p><code>appConfig.spring.datasource.username</code>
        </p>
      </td>
      <td style="text-align:left"><code>${superuser-username}</code>
      </td>
      <td style="text-align:left"><code>${postgresql-username}</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>evenstores.pg.persister.</code>
        </p>
        <p><code>appConfig.spring.datasource.url</code>
        </p>
      </td>
      <td style="text-align:left"><code>jdbc:postgresql://grnry-pg-citus:5432/postgres?currentSchema=public</code>
      </td>
      <td style="text-align:left"><code>jdbc:postgresql://grnry-pg:5432/postgres?currentSchema=public</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>harvester.default.metadata.</code>
        </p>
        <p><code>deploymentConfig.kubernetes.volumes</code>
        </p>
      </td>
      <td style="text-align:left"><code>&quot;[{name: &apos;secret&apos;, secret: { secretName : &apos;grnry-base-encryption-token&apos; , defaultMode : &apos;256&apos; }}, {name: &apos;db-secret&apos;, secret: { secretName : &apos;grnry-pg-citus-secret&apos; , defaultMode : &apos;256&apos; }}]&quot;</code>
      </td>
      <td style="text-align:left"><code>&quot;[{name: &apos;secret&apos;, secret: { secretName : &apos;grnry-base-encryption-token&apos; , defaultMode : &apos;256&apos; }}, {name: &apos;db-secret&apos;, secret: { secretName : &apos;grnry-pg-credentials&apos; , defaultMode : &apos;256&apos; }}]&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>harvester.default.transform.</code>
        </p>
        <p><code>deploymentConfig.kubernetes.volumes</code>
        </p>
      </td>
      <td style="text-align:left"><code>&quot;[{name: &apos;secret&apos;, secret: { secretName : &apos;grnry-base-encryption-token&apos; , defaultMode : &apos;256&apos; }}, {name: &apos;db-secret&apos;, secret: { secretName : &apos;grnry-pg-citus-secret&apos; , defaultMode : &apos;256&apos; }}]&quot;</code>
      </td>
      <td style="text-align:left"><code>&quot;[{name: &apos;secret&apos;, secret: { secretName : &apos;grnry-base-encryption-token&apos; , defaultMode : &apos;256&apos; }}, {name: &apos;db-secret&apos;, secret: { secretName : &apos;grnry-pg-credentials&apos; , defaultMode : &apos;256&apos; }}]&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>harvester.default.sessionizing.</code>
        </p>
        <p><code>deploymentConfig.kubernetes.volumes</code>
        </p>
      </td>
      <td style="text-align:left"><code>&quot;[{name: &apos;secret&apos;, secret: { secretName : &apos;grnry-base-encryption-token&apos; , defaultMode : &apos;256&apos; }}, {name: &apos;db-secret&apos;, secret: { secretName : &apos;grnry-pg-citus-secret&apos; , defaultMode : &apos;256&apos; }}]&quot;</code>
      </td>
      <td style="text-align:left"><code>&quot;[{name: &apos;secret&apos;, secret: { secretName : &apos;grnry-base-encryption-token&apos; , defaultMode : &apos;256&apos; }}, {name: &apos;db-secret&apos;, secret: { secretName : &apos;grnry-pg-credentials&apos; , defaultMode : &apos;256&apos; }}]&quot;</code>
      </td>
    </tr>
  </tbody>
</table>

### Presto

| Spec | Previous Default | Current default |
| :--- | :--- | :--- |
| `postgres.host` | `grnry-pg-citus` | `grnry-pg` |
| `postgres.secretName` | `grnry-pg-citus-secret` | `grnry-pg-credentials` |

### Kafka Profile Updater

| Spec | Previous default | Current default |
| :--- | :--- | :--- |
| `postgres.host` | `grnry-pg-citus` | `grnry-pg` |
| `postgres.secretName` | `grnry-pg-citus-secret` | `grnry-pg-credentials` |
| `postgres.secretKeyUsername` | `superuser-username` | `postgresql-username` |
| `postgres.secretKeyPassword` | `superuser-password` | `postgresql-password` |

### SCDF

| Spec | Previous Default | Current default |
| :--- | :--- | :--- |
| `postgres.host` | `grnry-pg-citus` | `grnry-pg` |
| `postgres.secretName` | `grnry-pg-citus-secret` | `grnry-pg-credentials` |
| `postgres.secretKeyUsername` | `superuser-username` | `postgresql-username` |
| `postgres.secretKeyPassword` | `superuser-password` | `postgresql-password` |

### Reaper

| Spec | Previous Default | Current default |
| :--- | :--- | :--- |
| `postgres.host` | `grnry-pg-citus` | `grnry-pg` |
| `postgres.secretName` | `grnry-pg-citus-secret` | `grnry-pg-credentials` |
| `postgres.secretKeyUsername` | `superuser-username` | `postgresql-username` |
| `postgres.secretKeyPassword` | `superuser-password` | `postgresql-password` |

### Eventstore Api

| Spec | Previous Default | Current default |
| :--- | :--- | :--- |
| `postgres.host` | `grnry-pg-citus` | `grnry-pg` |
| `postgres.secretName` | `grnry-pg-citus-secret` | `grnry-pg-credentials` |
| `postgres.secretKeyUsername` | `superuser-username` | `postgresql-username` |
| `postgres.secretKeyPassword` | `superuser-password` | `postgresql-password` |

### Profilestore Api

| Spec | Previous Default | Current default |
| :--- | :--- | :--- |
| `postgres.host` | `grnry-pg-citus` | `grnry-pg` |
| `postgres.secretName` | `grnry-pg-citus-secret` | `grnry-pg-credentials` |
| `postgres.secretKeyUsername` | `superuser-username` | `postgresql-username` |
| `postgres.secretKeyPassword` | `superuser-password` | `postgresql-password` |

### Belt Api

| Spec | Previous Default | Current default |
| :--- | :--- | :--- |
| `postgres.host` | `grnry-pg-citus` | `grnry-pg` |
| `postgres.secretName` | `grnry-pg-citus-secret` | `grnry-pg-credentials` |
| `postgres.secretKeyUsername` | `superuser-username` | `postgresql-username` |
| `postgres.secretKeyPassword` | `superuser-password` | `postgresql-password` |

### Event Feeder

| Spec | Previous Default | Current default |
| :--- | :--- | :--- |
| `postgres.host` | `grnry-pg-citus` | `grnry-pg` |
| `postgres.secretName` | `grnry-pg-citus-secret` | `grnry-pg-credentials` |
| `postgres.secretKeyUsername` | `superuser-username` | `postgresql-username` |
| `postgres.secretKeyPassword` | `superuser-password` | `postgresql-password` |

