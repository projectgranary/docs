---
description: On this page you get the technical details about source types.
---

# Available Source Types

Source types help you get data into the system. Source types exist for several different application types and are defined in the following. The source types mentioned here are the GRNRY specific source types. However, you may as well use the SCDF source types. For an overview of available source types you may refer to: [https://cloud.spring.io/spring-cloud-stream-app-starters/](https://cloud.spring.io/spring-cloud-stream-app-starters/)

{% hint style="info" %}
Refer to the [Granary Data-In Best Practices](../../../learning-grnry-1/data-in/best-practices-1/) to learn how to transform a source type's input to JSON format that is required by an Event Type.
{% endhint %}

### AWS S3 Source

**Name**: grnry-aws-s3

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

**Source parameters:**

The Amazon S3 Source is based on the Amazon S3 source provided by Spring Cloud Dataflow:

{% embed url="https://github.com/spring-cloud-stream-app-starters/aws-s3/tree/master/spring-cloud-starter-stream-source-s3" %}

Additionally, there are these source specific parameters:

| **Parameter** | Name |
| :--- | :--- |
| grnry-aws-s3.proxyHost | Host name when using a proxy. **\(String, default: `null`\)** |
| grnry-aws-s3.proxyPort | Service port when using a proxy. **\(Integer, default: `-1`\)** |
| grnry-aws-s3.nonProxyHosts | List of host names where a direct connection is possible. **\(String, default: `null`\)** |

A sample of the configuration of an Amazon S3 source could look like this:

```yaml
"sourceType" : {
    ...
    "configuration" : {
        "grnry-aws-s3.proxyHost" : "proxy.grnry.io",
        "grnry-aws-s3.proxyPort" : "1234",
        "file.consumer.mode": "lines",
        "s3.auto-create-local-dir": "true",
        "s3.remote-dir": "BUCKET_NAME",
        "s3.delete-remote-files": "true",
        "cloud.aws.credentials.access-key": "ACCESS_KEY",
        "cloud.aws.credentials.secret-key": "SECRET_KEY",
        "cloud.aws.region.static": "REGION",
        "cloud.aws.stack.auto": "false"
   }
}
```

### 

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
 "sourceType" : {
    ...
    "configuration" : {
      "sftp.factory.username": "<user>",
      "sftp.factory.password": "<Password>",
      "sftp.remote-dir": "./upload",
      "sftp.filename-pattern": "*.input",
      "sftp.trigger.fixed-delay": 20,
      "file.consumer.mode": "lines",
      "sftp.factory.allow-unknown-keys": true,
      "sftp.delete-remote-files": true,
      "sftp.factory.host": "<server-url>"
    }
 }
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



### Adobe Analytics Source \(S3\)

**Name**: grnry-adobe-s3

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

**Source parameters:**

The Adobe S3 Source is based on the AWS S3 source provided by Spring Cloud Dataflow. ****For AWS S3 parameters see above. Additionally, there are these source specific parameters:

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
  "sourceType" : {
    ...
    "configuration" : {
		 "file.consumer.mode": "ref",
			"s3.filename-pattern": "adobe/01-events*.gz",
			"s3.local-dir": "/adobe",
			"s3.remote-dir": "BUCKET_NAME",
			"s3.auto-create-local-dir": "true",
			"s3.delete-remote-files": "true",
			"cloud.aws.credentials.access-key": "ACCESS_KEY",
			"cloud.aws.credentials.secret-key": "SECRET_KEY",
			"cloud.aws.region.static": "REGION",
			"cloud.aws.stack.auto": "false",
			"cloud.aws.credentials.use-default-aws-credentials-chain" : "false" ,
			"grnry-adobe-s3.proxyHost": "proxyhost",
			"grnry-adobe-s3.proxyPort": "proxyport",
			"trigger.fixed-delay": "5",
			"trigger.time-unit": "SECONDS",
			"grnry-adobe-lookup.lookupTemplateString": "adobe/demo_${timestamp}-lookup_data.tar.gz",
			"grnry-adobe-lookup.localLookupFileTemplate": "adobe/lookup/${timestamp}-lookup",
			"grnry-adobe-lookup.lookupS3Bucket": "BUCKET_NAME"
   }
}
```

### 

### Adobe Analytics Source \(SFTP\)

**Name**: grnry-adobe-sftp

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

**Source parameters:**

The Adobe SFTP Source is based on the SFTP source provided by Spring Cloud Dataflow. ****For SFTP server parameters see above. Additionally, there are these source specific parameters:

| \*\*\*\* |
| :--- |


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
  "sourceType" : {
    ...
    "configuration" : {
		  "file.consumer.mode": "ref",
			"sftp.auto-create-local-dir": "true",
			"sftp.delete-remote-files": "true",
			"sftp.factory.allow-unknown-keys": "true",
			"sftp.factory.host": "sftp",
			"sftp.factory.password": "superpowers",
			"sftp.factory.port": "23",
			"sftp.factory.username": "myuser",
			"sftp.filename-pattern": "01-events*.gz",
			"sftp.stream": "false",
			"sftp.local-dir": "./sftp-test",
			"sftp.remote-dir": "./sftp-test",
			"trigger.fixed-delay": "5",
			"trigger.time-unit": "SECONDS",
			"grnry-adobe-lookup.lookupTemplateString": "sftp-test/demo_${timestamp}-lookup_data.tar.gz",
			"grnry-adobe-lookup.localLookupFileTemplate": "sftp-test/lookup/${timestamp}-lookup" 
   }
}
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
"sourceType" : {
    ...
    "configuration" : {
        "jdbc.max-rows-per-poll": 10000,
        "jdbc.query": "<select_statement>",
        "jdbc.update": "<insert_statement>",
        "spring.datasource.password": "<password>",
        "spring.datasource.url": "jdbc:postgresql://<url>:<port>/postgres?currentSchema=public",
        "spring.datasource.username": "<user>",
        "trigger.fixed-delay": 5,
        "trigger.time-unit": "SECONDS"
    }
}
```

### 

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
"sourceType" : {
    ...
    "configuration" : {
        "consumer.input-topic": "source-topic",
        "consumer.concurrency": "10",
        "consumer.startOffset": "earliest"
    }
}
```

### JMS Source

**Name**: grnry-jms

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

{% hint style="info" %}
Currently, there are no GRNRY-specific parameters for JMS. The benefit of this source type is the included IBM MQ libraray and the encryption of the processed messages.
{% endhint %}

**Source parameters:**

The parameters for the JMS source can be found here:

{% embed url="https://github.com/spring-cloud-stream-app-starters/jms/blob/master/spring-cloud-starter-stream-source-jms/README.adoc" %}

The JMS source also bundles IBM MQ JMS Spring Components to allow easy connection to IBM MQ endpoints. The IBM MQ related parameters can be found here:

{% embed url="https://github.com/ibm-messaging/mq-jms-spring/blob/master/README.md" %}

A sample of the configuration of a JMS source could look like this:

```yaml
"sourceType" : {
    ...
    "configuration" : {
        "jms.destination": "DEV.QUEUE.1",
        "ibm.mq.connName": "<host>(<port>)",
        "ibm.mq.user": "<user>",
        "ibm.mq.password": "<password>"
    }
}
```





{% hint style="warning" %}
All Source Types currently work only with one replica. Only exception is the Topic Source.
{% endhint %}

