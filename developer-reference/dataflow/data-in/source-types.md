---
description: On this page you get the technical details about source types.
---

# Available Source Types

Source types help you get data into the system. Source types exist for several different application types and are defined in the following. The source types mentioned here are the GRNRY specific source types. However, you may as well use the SCDF source types. For an overview of available source types you may refer to: [https://cloud.spring.io/spring-cloud-stream-app-starters/](https://cloud.spring.io/spring-cloud-stream-app-starters/)

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

### 

### HTTP Source

**Name**: grnry-http

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

{% hint style="info" %}
Currently, there are no GRNRY-specific parameters for HTTP. The benefit of this source type is encryption of the processed messages.
{% endhint %}

**Source parameters:**

The parameters for the HTTP source can be found here:

{% embed url="https://github.com/spring-cloud-stream-app-starters/http/blob/master/spring-cloud-starter-stream-source-http/README.adoc" caption="SCDF HTTP Source" %}



### Adobe Analytics Source

**Name**: grnry-adobe-sftp

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

**Source parameters:**

The Adobe SFTP Source is based on the SFTP source provided by Spring Cloud Dataflow. ****For SFTP server parameters see above.

| **Parameter** | Name |
| :--- | :--- |
| grnry-adobe-sftp.lookupTemplateString |  This is the path for the lookup files on the remote repository. The path must contain a placeholder for ${timestamp}. ${timestamp} evaluates to YYYYmmdd-HHiiss and is also taken from the path of the currently inspected file to retrieve the appropriate lookups from remote. **\(String, default: `sftpTarget/demo_${timestamp}-lookup_data.tar.gz)`** |
| grnry-adobe-sftp.localTemplateString |  This variable specifies where to store the files locally. It incorporates the timestamp \(${timestamp}\) again, as it is used to create folders for the different hours. The timestamp is written as YYYYmmdd-HHiiss **\(String, default: `tmp/lookup/${timestamp}-lookup`\)** |
| grnry-adobe-sftp.lookupsKeptInMemory |  Specifies how many past lookup files should be kept in memory. It might be the case that there are late arrivals of data which need to be reprocessed. However, the higher this value, the higher the memory consumption of the pod. If you provided a negative value or 0, there will be at max 10 lookups stored. **\(int, default: `1`\)** |
| grnry-adobe-sftp.deleteContentsOnDiskOnPriorityDelete | Defines whether the contents on disk are deleted, whenever the priority Queue entry is deleted as well. Meaning, whenever a lookup is removed from in-memory, the corresponding files on disk are removed as well. This should prevent high disk usage. Default is true. **\(Boolean, default: `true`\)** |
| grnry-adobe-sftp.listConfiguration | Defines the lists that should be resolved in the source type. It resolves lists within a column. The entries are splitted using \`--\`. Hence, if you want to define a list for column \`event\_list\`, which is separated by \`;\` you write \`event\_list--,\`. If you wanted to split more entries just add \`--\` and the next entry. **\(String, default: `event_list--,--post_event_list--,--mvvar2--|--post_mvvar2--|--product_list--;--post_product_list--;--mvvar3--|--post_mvvar3--|`\)** |
| grnry-adobe-sftp.lookupConfiguration |  Write the configuration for Lookups. Each entry consists of three parts: 1. The file name of the file to be looked up, 2. The source column, 3. The target column, The entries are separated by "," in itself. Several entries are separated by ";" **\(String, default: `browser.tsv,browser,browser_name;browser_type.tsv,browser,browser_type;color_depth.tsv,color,color_depth;connection_type.tsv,connection_type,connection_type_name;country.tsv,country,country_name;event.tsv,post_event_list,post_event_list_name;javascript_version.tsv,javascript,javascript_version;languages.tsv,language,language_name;operating_systems.tsv,os,os_name;referrer_type.tsv,ref_type,ref_type_name;resolution.tsv,resolution,resolution_name;search_engines.tsv,search_engine,search_engine_name;event.tsv,event_list,event_list_name`\)** |

A sample of the configuration of a Adobe Analytics source could look like this:

```yaml
	"app.grnry-adobe-sftp.file.consumer.mode": "lines",
	"app.grnry-adobe-sftp.file.consumer.with-markers": "false",
	"app.grnry-adobe-sftp.sftp.auto-create-local-dir": "true",
	"app.grnry-adobe-sftp.sftp.delete-remote-files": "true",
	"app.grnry-adobe-sftp.sftp.factory.allow-unknown-keys": "true",
	"app.grnry-adobe-sftp.sftp.factory.host": "<server-name>",
	"app.grnry-adobe-sftp.sftp.factory.password": "<server-pass>",
	"app.grnry-adobe-sftp.sftp.factory.port": "22",
	"app.grnry-adobe-sftp.sftp.factory.username": "<server-user>",
	"app.grnry-adobe-sftp.sftp.filename-pattern": "*.tsv",
	"app.grnry-adobe-sftp.sftp.stream" : "false",
	"app.grnry-adobe-sftp.sftp.local-dir": "/adobe",
	"app.grnry-adobe-sftp.sftp.remote-dir": "/adobe",
	"app.grnry-adobe-sftp.trigger.fixed-delay": "5",
	"app.grnry-adobe-sftp.trigger.time-unit": "SECONDS",
	"app.grnry-adobe-sftp.grnry-adobe-sftp.lookupTemplateString": "/adobe/demo_\\${timestamp}-lookup_data.tar.gz",
	"app.grnry-adobe-sftp.grnry-adobe-sftp.localTemplateString": "/adobe/lookup/\\${timestamp}-lookup",
	"app.grnry-adobe-sftp.grnry.harvesterName": "adobe-analytics-std-harvester",
	"app.grnry-adobe-sftp.grnry.eventTypeName": "adobe-analytics",

```

### 

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

### Topic Source

**Name:** grnry-topic

**Source parameters:**

| **Parameter** | Description |
| :--- | :--- |
| consumer.input-topic | The input Topic to read from. Default value: `snowplow`. |
| consumer.concurrency | The concurrency of the inbound consumer. Default value: `6`. |
| consumer.resetOffsets | Wether to reset offsets on the consumer to the value provided by startOffset. Default value: `false`. |
| consumer.startOffset | The starting offset for new groups. Allowed values: `earliest` and `latest`. Default value: `latest`. |

A sample of the configuration of a Topic source could look like this:

```yaml
"app.grnry-topic-source.consumer.input-topic": "source-topic",
"app.grnry-topic-source.consumer.concurrency": "10",
"app.grnry-topic-source.consumer.startOffset": "earliest"
```

### Amazon S3 Source

**Name**: grnry-aws-s3

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

**Source parameters:**

The Amazon S3 Source is based on the Amazon S3 source provided by Spring Cloud Dataflow. For non-granary-specific parameters see [here](https://github.com/spring-cloud-stream-app-starters/aws-s3/blob/master/spring-cloud-starter-stream-source-s3/README.adoc).

| **Parameter** | Name |
| :--- | :--- |
| grnry-aws-s3.proxyHost | Host name when using a proxy. **\(String, default: `null`\)** |
| grnry-aws-s3.proxyPort | Service port when using a proxy. **\(Integer, default: `-1`\)** |
| grnry-aws-s3.nonProxyHosts | List of host names where a direct connection is possible. **\(String, default: `null`\)** |

A sample of the configuration of an Amazon S3 source could look like this:

```yaml
	"app.grnry-aws-s3.grnry-aws-s3.proxyHost" : "proxy.grnry.io",
	"app.grnry-aws-s3.grnry-aws-s3.proxyPort" : "1234",
	"app.grnry-aws-s3.file.consumer.mode": "lines",
	"app.grnry-aws-s3.s3.auto-create-local-dir": "true",
	"app.grnry-aws-s3.s3.remote-dir": "BUCKET_NAME",
	"app.grnry-aws-s3.s3.delete-remote-files": "true",
	"app.grnry-aws-s3.cloud.aws.credentials.access-key": "ACCESS_KEY",
	"app.grnry-aws-s3.cloud.aws.credentials.secret-key": "SECRET_KEY",
	"app.grnry-aws-s3.cloud.aws.region.static": "REGION",
	"app.grnry-aws-s3.cloud.aws.stack.auto": "false"
```

