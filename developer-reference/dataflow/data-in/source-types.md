---
description: On this page you get the technical details about source types.
---

# Source Types

Source types help you get data into the system. Source types exist for several different application types and are defined in the following. The source types mentioned here are the GRNRY specific source types. However, you may as well use the SCDF source types. For an overview of available source types you may refer to: [https://cloud.spring.io/spring-cloud-stream-app-starters/](https://cloud.spring.io/spring-cloud-stream-app-starters/)

The different source types enable encryption of the data. In addition, there are two parameters you should always set:

### SFTP Source

**Name**: grnry-sftp

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

{% hint style="info" %}
Currently, there are no GRNRY-specific parameters for SFTP. The benefit of this source type is encryption of the processed messages.
{% endhint %}

**Source parameters:**

The parameters for the SFTP source can be found here:

{% embed url="https://github.com/spring-cloud-stream-app-starters/sftp/tree/master/spring-cloud-starter-stream-source-sftp" caption="Reference to the SCDF documentation for SFTP" %}

A sample for the configuration of a SFTP source could look like this:

```yaml
  "app.grnry-sftp.sftp.factory.username":"<user>",
  "app.grnry-sftp.sftp.factory.password":"<Password>",
  "app.grnry-sftp.sftp.remote-dir":"./upload",
  "app.grnry-sftp.sftp.filename-pattern":"*.input",
  "app.grnry-sftp.sftp.trigger.fixed-delay": 20,
  "app.grnry-sftp.file.consumer.mode": "lines",
  "app.grnry-sftp.sftp.factory.allow-unknown-keys": true,
  "app.grnry-sftp.sftp.delete-remote-files": true,
  "app.grnry-sftp.sftp.factory.host": "<server-url>",
  "app.grnry-sftp.grnry.harvesterName":"sftp-all-std-harvester",
  "app.grnry-sftp.grnry.eventTypeName":"sftp-all",
```

### HTTP Source

**Name**: grnry-http

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

{% hint style="info" %}
Currently, there are no GRNRY-specific parameters for HTTP. The benefit of this source type is encryption of the processed messages.
{% endhint %}

**Source parameters:**

The parameters for the HTTP source can be found here:

{% embed url="https://github.com/spring-cloud-stream-app-starters/http/blob/master/spring-cloud-starter-stream-source-http/README.adoc" caption="SCDF HTTP Source" %}

### JDBC Source

**Name**: grnry-jdbc

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

{% hint style="info" %}
Currently, there are no GRNRY-specific parameters for JDBC. The benefit of this source type is encryption of the processed messages.
{% endhint %}

**Source parameters:**

The parameters for the JDBC source can be found here:

{% embed url="https://github.com/spring-cloud-stream-app-starters/jdbc/blob/master/spring-cloud-starter-stream-source-jdbc/README.adoc" %}

A sample of the configuration of a JDBC source could look like this:

```yaml
   "app.grnry-jdbc.jdbc.max-rows-per-poll":10000,
   "app.grnry-jdbc.jdbc.query":"<select_statement>",
   "app.grnry-jdbc.jdbc.update":"<insert_statement>",
   "app.grnry-jdbc.spring.datasource.password":"<password>",
   "app.grnry-jdbc.spring.datasource.url":"jdbc:postgresql://<url>:<port>/postgres?currentSchema=public",
   "app.grnry-jdbc.spring.datasource.username":"<user>",
   "app.grnry-jdbc.trigger.fixed-delay":5,
   "app.grnry-jdbc.trigger.time-unit":"SECONDS",
   "app.grnry-jdbc.grnry.harvesterName":"jdbc-std-harvester",
   "app.grnry-jdbc.grnry.eventTypeName":"jdbc",
```

