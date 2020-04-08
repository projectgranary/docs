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
  "app.grnry-sftp.sftp.factory.username": "<user>",
  "app.grnry-sftp.sftp.factory.password": "<Password>",
  "app.grnry-sftp.sftp.remote-dir": "./upload",
  "app.grnry-sftp.sftp.filename-pattern": "*.input",
  "app.grnry-sftp.sftp.trigger.fixed-delay": 20,
  "app.grnry-sftp.file.consumer.mode": "lines",
  "app.grnry-sftp.sftp.factory.allow-unknown-keys": true,
  "app.grnry-sftp.sftp.delete-remote-files": true,
  "app.grnry-sftp.sftp.factory.host": "<server-url>",
  "app.grnry-sftp.grnry.harvesterName": "sftp-std-harvester",
  "app.grnry-sftp.grnry.eventTypeName": "sftp",
```



### AWS S3 Source

**Name**: grnry-aws-s3

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

**Source parameters:**

The AWS S3 Source is based on the Amazon S3 source provided by Spring Cloud Dataflow:

{% embed url="https://github.com/spring-cloud-stream-app-starters/aws-s3/tree/master/spring-cloud-starter-stream-source-s3" %}

Additionally, there are these source specific parameters:

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
	"app.grnry-aws-s3.cloud.aws.stack.auto": "false",
  "app.grnry-aws-s3.grnry.harvesterName": "s3-std-harvester",
  "app.grnry-aws-s3.grnry.eventTypeName": "s3",
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



### Adobe Analytics Source \(SFTP\)

**Name**: grnry-adobe-sftp

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

**Source parameters:**

The Adobe SFTP Source is based on the SFTP source provided by Spring Cloud Dataflow. ****For SFTP server parameters see above. Additionally, there are these source specific parameters:

| **Parameter** | Name |
| :--- | :--- |
| grnry-adobe-lookup.lookupTemplateString |  This is the path for the lookup files on the remote repository. The path must contain a placeholder for ${timestamp}. ${timestamp} evaluates to YYYYmmdd-HHiiss and is also taken from the path of the currently inspected file to retrieve the appropriate lookups from remote. **\(String, default: `null`\)** |
| grnry-adobe-lookup.localLookupDirectory |  This variable specifies where to store the files locally. It incorporates the timestamp \(${timestamp}\) again, as it is used to create folders for the different hours. The timestamp is written as YYYYmmdd-HHiiss **\(String, default: `tmp/lookup/`\)** |
| grnry-adobe-lookup.localLookupFileTemplate |  The template String for the local storage of the files. Ensure that you have at least one folder in the hierarchy here. **\(String, default:  `${timestamp}-lookup`\)** |
| grnry-adobe-lookup.lookupsKeptInMemory |  Specifies how many past lookup files should be kept in memory. It might be the case that there are late arrivals of data which need to be reprocessed. However, the higher this value, the higher the memory consumption of the pod. If you provided a negative value or 0, there will be at max 10 lookups stored. **\(int, default: `1`\)** |
| grnry-adobe-lookup.listConfiguration | Defines the lists that should be resolved in the source type. It resolves lists within a column. The entries are splitted using \`--\`. Hence, if you want to define a list for column \`event\_list\`, which is separated by \`;\` you write \`event\_list--,\`. If you wanted to split more entries just add \`--\` and the next entry. **\(String, default: `event_list--,--post_event_list--,--mvvar2--|--post_mvvar2--|--product_list--;--post_product_list--;--mvvar3--|--post_mvvar3--|`\)** |
| grnry-adobe-lookup.lookupConfiguration |  Write the configuration for Lookups. Each entry consists of three parts: 1. The file name of the file to be looked up, 2. The source column, 3. The target column, The entries are separated by "," in itself. Several entries are separated by ";" **\(String, default: `browser.tsv,browser,browser_name;browser_type.tsv,browser,browser_type;color_depth.tsv,color,color_depth;connection_type.tsv,connection_type,connection_type_name;country.tsv,country,country_name;event.tsv,post_event_list,post_event_list_name;javascript_version.tsv,javascript,javascript_version;languages.tsv,language,language_name;operating_systems.tsv,os,os_name;referrer_type.tsv,ref_type,ref_type_name;resolution.tsv,resolution,resolution_name;search_engines.tsv,search_engine,search_engine_name;event.tsv,event_list,event_list_name`\)** |

A sample of the configuration of a Adobe Analytics source could look like this:

```yaml
	"app.grnry-adobe-sftp.file.consumer.mode": "ref",
	"app.grnry-adobe-sftp.sftp.auto-create-local-dir": "true",
	"app.grnry-adobe-sftp.sftp.delete-remote-files": "true",
	"app.grnry-adobe-sftp.sftp.factory.allow-unknown-keys": "true",
	"app.grnry-adobe-sftp.sftp.factory.host": "<server-name>",
	"app.grnry-adobe-sftp.sftp.factory.password": "<server-pass>",
	"app.grnry-adobe-sftp.sftp.factory.port": "22",
	"app.grnry-adobe-sftp.sftp.factory.username": "<server-user>",
	"app.grnry-adobe-sftp.sftp.filename-pattern": "01-events*.gz",
	"app.grnry-adobe-sftp.sftp.stream" : "false",
	"app.grnry-adobe-sftp.sftp.local-dir": "/adobe",
	"app.grnry-adobe-sftp.sftp.remote-dir": "/adobe",
	"app.grnry-adobe-sftp.trigger.fixed-delay": "5",
	"app.grnry-adobe-sftp.trigger.time-unit": "SECONDS",
	"app.grnry-adobe-sftp.grnry-adobe-lookup.lookupTemplateString": "/adobe/demo_\\${timestamp}-lookup_data.tar.gz",
	"app.grnry-adobe-sftp.grnry-adobe-lookup.localLookupFileTemplate": "/adobe/lookup/\\${timestamp}-lookup",
	"app.grnry-adobe-sftp.grnry.harvesterName": "adobe-analytics-sftp-std-harvester",
	"app.grnry-adobe-sftp.grnry.eventTypeName": "adobe-analytics",
```

### 

### Adobe Analytics Source \(S3\)

**Name**: grnry-adobe-s3

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

**Source parameters:**

The Adobe S3 Source is based on the AWS S3 source provided by Spring Cloud Dataflow. ****For AWS S3 bucket parameters see above. Additionally, there are these source specific parameters:

| **Parameter** | Name |
| :--- | :--- |
| grnry-adobe-lookup.lookupTemplateString |  This is the path for the lookup files on the remote repository. The path must contain a placeholder for ${timestamp}. ${timestamp} evaluates to YYYYmmdd-HHiiss and is also taken from the path of the currently inspected file to retrieve the appropriate lookups from remote. **\(String, default: `null`\)** |
| grnry-adobe-lookup.localLookupDirectory |  This variable specifies where to store the files locally. It incorporates the timestamp \(${timestamp}\) again, as it is used to create folders for the different hours. The timestamp is written as YYYYmmdd-HHiiss **\(String, default: `tmp/lookup/`\)** |
| grnry-adobe-lookup.localLookupFileTemplate |  The template String for the local storage of the files. Ensure that you have at least one folder in the hierarchy here. **\(String, default:  `${timestamp}-lookup`\)** |
| grnry-adobe-lookup.lookupsKeptInMemory |  Specifies how many past lookup files should be kept in memory. It might be the case that there are late arrivals of data which need to be reprocessed. However, the higher this value, the higher the memory consumption of the pod. If you provided a negative value or 0, there will be at max 10 lookups stored. **\(int, default: `1`\)** |
| grnry-adobe-lookup.listConfiguration | Defines the lists that should be resolved in the source type. It resolves lists within a column. The entries are splitted using \`--\`. Hence, if you want to define a list for column \`event\_list\`, which is separated by \`;\` you write \`event\_list--,\`. If you wanted to split more entries just add \`--\` and the next entry. **\(String, default: `event_list--,--post_event_list--,--mvvar2--|--post_mvvar2--|--product_list--;--post_product_list--;--mvvar3--|--post_mvvar3--|`\)** |
| grnry-adobe-lookup.lookupConfiguration |  Write the configuration for Lookups. Each entry consists of three parts: 1. The file name of the file to be looked up, 2. The source column, 3. The target column, The entries are separated by "," in itself. Several entries are separated by ";" **\(String, default: `browser.tsv,browser,browser_name;browser_type.tsv,browser,browser_type;color_depth.tsv,color,color_depth;connection_type.tsv,connection_type,connection_type_name;country.tsv,country,country_name;event.tsv,post_event_list,post_event_list_name;javascript_version.tsv,javascript,javascript_version;languages.tsv,language,language_name;operating_systems.tsv,os,os_name;referrer_type.tsv,ref_type,ref_type_name;resolution.tsv,resolution,resolution_name;search_engines.tsv,search_engine,search_engine_name;event.tsv,event_list,event_list_name`\)** |
| grnry-adobe-lookup.lookupS3Bucket |  S3 bucket where to find lookup files. **\(String, default: `null`\)** |

A sample of the configuration of a Adobe Analytics source could look like this:

```yaml
  "app.grnry-adobe-s3.file.consumer.mode": "ref",
  "app.grnry-adobe-s3.s3.filename-pattern": "adobe/01-events*.gz",
  "app.grnry-adobe-s3.s3.local-dir": "/adobe",
  "app.grnry-adobe-s3.s3.remote-dir": "BUCKET_NAME",
  "app.grnry-adobe-s3.s3.auto-create-local-dir": "true",
  "app.grnry-adobe-s3.s3.delete-remote-files": "true",
  "app.grnry-adobe-s3.cloud.aws.credentials.access-key": "ACCESS_KEY",
  "app.grnry-adobe-s3.cloud.aws.credentials.secret-key": "SECRET_KEY",
  "app.grnry-adobe-s3.cloud.aws.region.static": "REGION",
  "app.grnry-adobe-s3.cloud.aws.stack.auto": "false",
  "app.grnry-adobe-s3.cloud.aws.credentials.use-default-aws-credentials-chain" : "false" ,
  "app.grnry-adobe-s3.grnry-adobe-s3.proxyHost": "proxyhost",
  "app.grnry-adobe-s3.grnry-adobe-s3.proxyPort": "proxyport",
  "app.grnry-adobe-s3.trigger.fixed-delay": "5",
  "app.grnry-adobe-s3.trigger.time-unit": "SECONDS",
  "app.grnry-adobe-s3.grnry-adobe-lookup.lookupTemplateString": "adobe/demo_${timestamp}-lookup_data.tar.gz",
  "app.grnry-adobe-s3.grnry-adobe-lookup.localLookupFileTemplate": "adobe/lookup/${timestamp}-lookup",
  "app.grnry-adobe-s3.grnry-adobe-lookup.lookupS3Bucket": "BUCKET_NAME"
	"app.grnry-adobe-s3.grnry.harvesterName": "adobe-analytics-s3-std-harvester",
	"app.grnry-adobe-s3.grnry.eventTypeName": "adobe-analytics",
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
   "app.grnry-jdbc.jdbc.max-rows-per-poll": 10000,
   "app.grnry-jdbc.jdbc.query": "<select_statement>",
   "app.grnry-jdbc.jdbc.update": "<insert_statement>",
   "app.grnry-jdbc.spring.datasource.password": "<password>",
   "app.grnry-jdbc.spring.datasource.url": "jdbc:postgresql://<url>:<port>/postgres?currentSchema=public",
   "app.grnry-jdbc.spring.datasource.username": "<user>",
   "app.grnry-jdbc.trigger.fixed-delay": 5,
   "app.grnry-jdbc.trigger.time-unit": "SECONDS",
   "app.grnry-jdbc.grnry.harvesterName": "jdbc-std-harvester",
   "app.grnry-jdbc.grnry.eventTypeName": "jdbc",
```

