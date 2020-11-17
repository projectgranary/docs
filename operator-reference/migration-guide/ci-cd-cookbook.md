---
description: Continous integration / delivery migration guide
---

# Harvester API CI/CD Cookbook

## pre-GRNRY 0.8 SCDF-API based CD of harvesters

The pre-GRNRY 0.8 CD pipeline is creating raw SCDF streams. A devops person is responsible to configure all moving parts accordingly so that data is passed on correctly and common configuration options are matching the harvester scope \(like names, extraction rules\) but also environment \(like encryption keys, database secret names\). It consists of two SCDF stream definitions and configurations and the CI/CD interaction with SCDF will drop streams \(if they exists\) and afterwards create and deploy them on each update.

### Flow:

1. delete stream if exist \(will undeploy it implicitly\)
2. \(re-\)create stream \(stream definition is used, see below\)
3. deploy stream \(stream configuration is used, see below\)

### Stream definitions \(example\)

```text
Harvester stream:
sink: grnry-jdbc | trans: grnry-transform | meta: grnry-metadata-extractor > grnry_data_in_ora-abc

Persister stream:
grnry_data_in_ora-abc > sink: grnry-pg-eventstore
```

### Persister Stream configuration \(example\)

```text
{
	"app.grnry-eventstore-pg.spring.cloud.kubernetes.secrets.paths": "/usr/src/app/db-secret",
	"app.grnry-eventstore-pg.spring.datasource.password": "${superuser-password}",
	"app.grnry-eventstore-pg.spring.datasource.username": "${superuser-username}",
	"app.grnry-eventstore-pg.eventstore.tableName": "public.eventstore",
	"app.grnry-eventstore-pg.spring.datasource.url": "jdbc:postgresql://grnry-pg-citus:5432/postgres?currentSchema=kafka_connnect",
	"app.grnry-eventstore-pg.spring.cloud.stream.bindings.input.consumer.concurrency": 6,
	"app.grnry-eventstore-pg.spring.cloud.stream.bindings.input.consumer.partitioned": true,
	"deployer.*.kubernetes.imagePullPolicy": "Always",
	"deployer.*.kubernetes.limits.cpu": "400m",
	"deployer.*.kubernetes.limits.memory": "512Mi",
	"deployer.*.kubernetes.requests.cpu": "400m",
	"deployer.*.kubernetes.requests.memory": "512Mi",
	"deployer.*.kubernetes.livenessProbeDelay": "120",
	"deployer.*.kubernetes.readinessProbeDelay": "120",
	"deployer.*.kubernetes.volumeMounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
	"deployer.*.kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]"
}
```

### Harvester Stream configuration \(jdbc example\)

```text
{
   "app.source.jdbc.max-rows-per-poll":10000,
   "app.source.jdbc.query":"select t.id,t.kunde_nr,t.vertrag_nr,t.dokument from jdbc_import_table as t, jdbc_import_table_status tstate where t.id > tstate.last_seen_id order by t.id asc",
   "app.source.jdbc.update":"insert into jdbc_import_table_status (dummy_id , last_seen_id) values (1 , GREATEST( :id ) ) ON CONFLICT (dummy_id) DO UPDATE SET last_seen_id = GREATEST(jdbc_import_table_status.last_seen_id , EXCLUDED.last_seen_id)",
   "app.source.spring.datasource.password":"jdbc_import_password",
   "app.source.spring.datasource.url":"jdbc:postgresql://grnry-pg-citus:5432/postgres?currentSchema=public",
   "app.source.spring.datasource.username":"jdbc_import_user",
   "app.source.trigger.fixed-delay":5,
   "app.source.trigger.time-unit":"SECONDS",
   "app.source.grnry.harvesterName":"ora-abc-std-harvester",
   "app.source.grnry.eventTypeName":"ora-abc",
   "app.source.spring.cloud.stream.kafka.binder.autoCreateTopics" : true,
   "app.source.spring.cloud.stream.kafka.binder.autoAddPartitions" : true,
   "app.trans.scriptable-transformer.language":"groovy",
   "app.trans.scriptable-transformer.script":"return new String(payload, 'UTF-8')",
   "app.trans.grnry.harvesterName":"ora-abc-std-harvester",
   "app.trans.grnry.eventTypeName":"ora-abc",
   "app.trans.spring.cloud.stream.bindings.input.consumer.concurrency":3,
   "app.trans.spring.cloud.stream.bindings.input.consumer.partitioned":true,
   "app.trans.spring.cloud.stream.bindings.output.consumer.partitioned":true,
   "app.trans.spring.cloud.stream.bindings.output.producer.partitionCount":24,
   "app.trans.spring.cloud.stream.kafka.binder.autoCreateTopics" : true,
   "app.trans.spring.cloud.stream.kafka.binder.autoAddPartitions" : true,
   "app.trans.spring.cloud.stream.kafka.bindings.input.consumer.resetOffsets":true,
   "app.trans.spring.cloud.stream.kafka.bindings.input.consumer.startOffset":"earliest",
   "app.trans.spring.cloud.stream.bindings.harvesterdlq.destination": "grnry_harvester_${grnry.harvesterName}_dlq",
   "app.trans.spring.cloud.stream.bindings.harvesterdlq.producer.partitionCount": 24,
   "app.trans.spring.cloud.stream.bindings.harvesterdlq.producer.autoAddPartitions": true,
   "app.meta.metadata.correlationIdExpression":"#safeJsonPath(payload, 'kunde_nr')?:'JDBC_NO_CORRELATION_ID'",
   "app.meta.metadata.eventIdExpression":"#safeJsonPath(payload, 'vertrag_nr')?:'JDBC_NO_EVENT_ID'",
   "app.meta.metadata.eventTypeName":"${grnry.eventTypeName}",
   "app.meta.metadata.harvesterName":"${grnry.harvesterName}",
   "app.meta.spring.cloud.stream.bindings.input.consumer.concurrency":3,
   "app.meta.spring.cloud.stream.bindings.input.consumer.partitioned":true,
   "app.meta.spring.cloud.stream.kafka.binder.autoCreateTopics" : true,
   "app.meta.spring.cloud.stream.kafka.binder.autoAddPartitions" : true,
   "app.meta.grnry.harvesterName":"abc-std-harvester",
   "app.meta.grnry.eventTypeName":"ora-abc",
   "app.meta.spring.cloud.stream.bindings.harvesterdlq.destination": "grnry_harvester_${grnry.harvesterName}_dlq",
   "app.meta.spring.cloud.stream.bindings.harvesterdlq.producer.partitionCount": 24,
   "app.meta.spring.cloud.stream.bindings.harvesterdlq.producer.autoAddPartitions": true,
   "deployer.*.kubernetes.imagePullPolicy":"Always",
   "deployer.*.kubernetes.limits.cpu":"400m",
   "deployer.*.kubernetes.limits.memory":"512Mi",
   "deployer.*.kubernetes.requests.cpu":"400m",
   "deployer.*.kubernetes.requests.memory":"512Mi",
   "deployer.*.kubernetes.livenessProbeDelay":"120",
   "deployer.*.kubernetes.readinessProbeDelay":"120",
   "deployer.*.kubernetes.volumeMounts":"[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }]",
   "deployer.*.kubernetes.volumes":"[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}]"
}
```



### streamDefine.groovy

```text
def call(Map params) {

    String scdfServerCredentials = params.get('scdfServerCredentials')
    String scdfServerUrl = params.get('scdfServerUrl')
    String stream = params.get('stream')
    String definition = params.get('definition')
    String debug = params.get('debug', '-f').replaceAll("(?i)true", "-v");

    writeFile file: env.WORKSPACE + "/" + stream + "-def.tmp.json", text: definition
    sh "curl -X POST ${debug} -u ${scdfServerCredentials} ${scdfServerUrl}/streams/definitions?name=${stream} --data-urlencode definition@$WORKSPACE/${stream}-def.tmp.json"
}

```

### streamDelete.groovy

```text
def call(Map params) {

    String scdfServerCredentials = params.get('scdfServerCredentials')
    String scdfServerUrl = params.get('scdfServerUrl')
    String stream = params.get('stream')
    String ignoreError = params.get('ignoreError', '-f').replaceAll("(?i)true", "");
    String debug = params.get('debug', ignoreError).replaceAll("(?i)true", "-v");

    sh "curl -X DELETE ${debug} -u ${scdfServerCredentials} ${scdfServerUrl}/streams/deployments/${stream}"
    sh "curl -X DELETE ${debug} -u ${scdfServerCredentials} ${scdfServerUrl}/streams/definitions/${stream}"
}

```

### streamDeploy.groovy

```text
def call(Map params) {

    String scdfServerCredentials = params.get('scdfServerCredentials')
    String scdfServerUrl = params.get('scdfServerUrl')
    String stream = params.get('stream')
    String configFile = params.get('configFile', stream + '.json')
    String debug = params.get('debug', '-f').replaceAll("(?i)true", "-v");

    sh "curl -X POST ${debug} -u ${scdfServerCredentials} ${scdfServerUrl}/streams/deployments/${stream} -d @${configFile} -H 'Content-Type: application/json'  -H 'cache-control: no-cache'"
}

```

## GRNRY 0.8 Harvester API based CD of event types and harvesters

The "pre-GRNRY 0.8" approach is just operating on streams, which are safe to destroy and recreate because the used kafka topics \(and consumer groups\) are not removed. 

Recreating harvesters and especially event types by deleting them is not recommended, because it might also drop already imported data and might fail if other components \(e.g. belts\) are still using them.

{% hint style="info" %}
This guide does not cover the initial configuration of source types. It expects that all necessary source types are already registered and preconfigured with the necessary environment-specific application and deployment configuration properties.
{% endhint %}

### Event type definition 

The example event type specific configuration properties in our example look like this:

```text
....
"app.meta.metadata.correlationIdExpression":"#safeJsonPath(payload, 'kunde_nr')?:'JDBC_NO_CORRELATION_ID'",
"app.meta.metadata.eventIdExpression":"#safeJsonPath(payload, 'vertrag_nr')?:'JDBC_NO_EVENT_ID'",
...
"app.meta.grnry.eventTypeName":"ora-abc",   
...
```

Map `correlationIdExpression`, `eventIdExpression` \(and `timestampExpression` if set\) to event-type entity properties and use `app.meta.grnry.eventTypeName` value as name and create a file named `event-type.json` that will be used in the api calls to create/update the event type:

{% code title="event-type.json" %}
```text
{
  "displayName" : "ora-abc",
  "correlationIdExpression" : "#safeJsonPath(payload, 'kunde_nr')?:'JDBC_NO_CORRELATION_ID'",
  "eventIdExpression" : "#safeJsonPath(payload, 'vertrag_nr')?:'JDBC_NO_EVENT_ID'"
}
```
{% endcode %}

{% hint style="info" %}
The technical `name` \(used in URIs\)  of the event type is derived from the `displayName`. Because you need to know the technical name in your ci/cd pipeline to check for already existing entities \(do i have to create it ? Do i have to update an already existing entity ?\) it is recommend to use displayNames in an CI/CD driven and automated environment that can be used 1:1 as the technical name. The rules are

* only use a-z , A-Z , 0-9 and "-" \(dash\)
* do not exceed 40 chars
{% endhint %}

### Create / Update Event Type

First check if the event-type \( `event-type name: ora-abc` \) already exists:

```text
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 https:/grnry-host/event-types/ora-abc
```

 if it's not found \(`404`\): create it with

```text
curl -X POST -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @event-type.json \
 https:/grnry-host/event-types
```

  or if it's already available `(200 SUCCESS)` update the entity with

```text
curl -X PUT -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @event-type.json \
  https:/grnry-host/event-types/ora-abc
```

The update may return a `200 (SUCCESS)` with the current definition of the event type in case values were updated or a `304 (NOT MODIFIED)` without any content in case the current properties for `correlationIdExpression` etc are equal to the values in your update.

{% hint style="info" %}
It is safe to use a partial event type definition in the update call. The api will only update/check provided properties and will leave the rest unchanged. Therefore it is ok to do the update "blindly" without GET'ing the full event-type entity + updating this entity before using it during the update.
{% endhint %}

### Persister State Management

On the initial creation of the event type a persister is also created, but not started. The configuration of the persister looks similar to the "pre-harvester-api" Persister Stream definition. But all environment specific  configuration options \(database host, username, encryption key locations, etc\) are already available as defaults in the harvester-api deployment \(via helm\). This tutorial will assume that you do not need to adapt the default settings \(which is still possible, please consult the [api docs](../../developer-reference/api-reference/harvester-api/#event-type-endpoints) \).

Check the current state of the persister

```text
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
  https:/grnry-host/event-types/ora-abc/eventstores/pg/persister/state
```

Example response:

```text
{
    "status": "STOPPED",
    "_links": {
        "self": {
            "href": "https:/grnry-host/event-types/ora-abc/eventstores/pg/persister/state"
        }
    }
}
```

| State | Description |
| :--- | :--- |
| `UNKNOWN` | status could not be determined |
| `STOPPED` | persister is not deployed |
| `DEPLOYING` | persister is being deployed |
| `STOPPING` | persister is being stopped |
| `RUNNING` | persister is running |
| `RUNNING_BUT_OUTDATED` | persister is running with an outdated configuration, because it was updated after startup. For these changes to become effective the persister has to be stopped and started |
| `FAILED` | persister is deployed but not running because of other failures \(application startup, insufficient k8s resources, ...\) |

if stopped, start the persister:

```text
curl -X POST -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @start.json \  
  https:/grnry-host/event-types/ora-abc/persister/pg/state
```

using

{% code title="start.json" %}
```text
{
  "action" : "START" 
}
```
{% endcode %}

Then wait and re-check until the persister is running. This might take some time depending on your settings regarding the liveliness probe intervals.

```text
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
  https:/grnry-host/event-types/ora-abc/persister/pg/state
```

{% hint style="info" %}
Once started there is no need to start and stop the persister on each deployment as they   
do not contain any updateable event type - specific configuration  \(like `correlationIdExpression,`etc\). Start/Stop is necessary if you modify the persister configuration \(deployment options like cpu/ram\), which would result in a `RUNNING_BUT_OUTDATE`state.  

So as a general rule the CI/CD script can do nothing on `RUNNING`but stop and start the persister on `RUNNING_BUT_OUTDATED` .
{% endhint %}

### Harvester Instance Definition

Event Type Configuration

As you already migrated the correlationId etc expressions, you just need to reference the event type by name and version in your harvester definition. You can use the version alias "latest" or an explict version number:

```text
"eventType": {
  "name": "ora-abc",
  "version": "latest"  // or explict version like 1
}
```

#### Source Type Configuration

You need to reference an already available source implementation:

```text
"sourceType": {
    ...
    "name": "grnry-jdbc",
    "version": "latest",
    ...
}
```

Now we migrate the individual settings for each stage.

**Migrating "app.source" settings**

You values you need to migrate are all prefixed with `app.source`. These are the lines from our example:

```text
{
...
   "app.source.jdbc.max-rows-per-poll":10000,
   "app.source.jdbc.query":"select t.id,t.kunde_nr,t.vertrag_nr,t.dokument from jdbc_import_table as t, jdbc_import_table_status tstate where t.id > tstate.last_seen_id order by t.id asc",
   "app.source.jdbc.update":"insert into jdbc_import_table_status (dummy_id , last_seen_id) values (1 , GREATEST( :id ) ) ON CONFLICT (dummy_id) DO UPDATE SET last_seen_id = GREATEST(jdbc_import_table_status.last_seen_id , EXCLUDED.last_seen_id)",
   "app.source.spring.datasource.password":"jdbc_import_password",
   "app.source.spring.datasource.url":"jdbc:postgresql://grnry-pg-citus:5432/postgres?currentSchema=public",
   "app.source.spring.datasource.username":"jdbc_import_user",
   "app.source.trigger.fixed-delay":5,
   "app.source.trigger.time-unit":"SECONDS",
   "app.source.grnry.harvesterName":"ora-abc-std-harvester",
   "app.source.grnry.eventTypeName":"ora-abc",
   "app.source.spring.cloud.stream.kafka.binder.autoCreateTopics" : true,
   "app.source.spring.cloud.stream.kafka.binder.autoAddPartitions" : true,
...
}
```

You can ignore grnry.harvesterName and grnry.eventTypeName, but all other properties **not starting** with spring.cloud.stream has to be placed in `sourceType/configuration` \(Without the `app.source` prefix\)

```text
{
  "sourceType": {
   ...
    "configuration": {
     "jdbc.max-rows-per-poll":10000,
     "jdbc.query":"select t.id,t.kunde_nr,t.vertrag_nr,t.dokument from jdbc_import_table as t, jdbc_import_table_status tstate where t.id > tstate.last_seen_id order by t.id asc",
     "jdbc.update":"insert into jdbc_import_table_status (dummy_id , last_seen_id) values (1 , GREATEST( :id ) ) ON CONFLICT (dummy_id) DO UPDATE SET last_seen_id = GREATEST(jdbc_import_table_status.last_seen_id , EXCLUDED.last_seen_id)",
     "spring.datasource.password":"jdbc_import_password",
     "spring.datasource.url":"jdbc:postgresql://grnry-pg-citus:5432/postgres?currentSchema=public",
     "spring.datasource.username":"jdbc_import_user",
     "trigger.fixed-delay":5,
     "trigger.time-unit":"SECONDS",
    },
....
},
```

All properties **starting with** `spring.cloud.stream` can be placed in sourceType/appConfiguration :

{% hint style="info" %}
Each registered source type definition contains a default configuration block for `sourceType/appConfiguration`, which should contains the enviroment specific settings. Be aware that IF you define `sourceType/appConfiguration` you need to provide all configuration options. It will not be merged with the default values.
{% endhint %}

```text
{
  "sourceType": {
   ...
    "appConfiguration": {
     "app.source.spring.cloud.stream.kafka.binder.autoCreateTopics" : true,
     "app.source.spring.cloud.stream.kafka.binder.autoAddPartitions" : true,
    },
....
},
```

If your scdf stream definition contains app specific deployment settings like

```text
  "deployer.*.kubernetes.limits.cpu":"400m",
  "deployer.*.kubernetes.limits.memory":"512Mi",
  "deployer.*.kubernetes.requests.cpu":"400m",
 ... source specific: ...
   "deployer.source.kubernetes.limits.cpu":"400m",
   "deployer.source.kubernetes.limits.memory":"512Mi",
   "deployer.source.kubernetes.requests.cpu":"400m",
 
```

you can place them in `sourceType/deploymentConfiguration`:

```text
 "sourceType": {
   "name": "grnry-jdbc",
   "version": "latest",
   "configuration": {},
   "deploymentConfiguration": {
       ....
       "kubernetes.limits.cpu": "400m",
       "kubernetes.limits.memory": "512Mi",
       "kubernetes.requests.cpu": "400m",
       ...
   },
 }
```

{% hint style="info" %}
Each registered source type definition contains a default configuration block for `sourceType/deploymentConfiguration,` which should contains the environment specific settings. Be aware that IF you define `sourceType/deploymentConfiguration` you need to provide all configuration options. It will not be merged with the default values if set.
{% endhint %}

#### Transform Configuration

You need to migration the language and the transform script from your SCDF stream definition:

```text
 "app.trans.scriptable-transformer.language":"groovy",
 "app.trans.scriptable-transformer.script":"return new String(payload, 'UTF-8')",
```

and Insert them as-is into the `transform` section:

```text
 "transform": {
   ...
   "language": "groovy",
   "script": "return new String(payload , 'UTF-8');"
   ...
 }
```

The transform configuration also provides optional sections `transform/appConfiguration` and `transform/deploymentConfiguration` to migrate spring cloud stream and deployment specific configuration options. But the harvester-api will use environment-specific default values that should work in most cases if you do not define them at all. 

Example for custom `appConfiguration` and `deploymentConfiguration`:

```text
"transform": {
    ...
    "deploymentConfiguration": {
        "kubernetes.imagepullpolicy": "Always",
     ....
        "kubernetes.requests.memory": "512Mi",
    },
    "appConfiguration": {
        "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
        ...
        "spring.cloud.stream.kafka.binder.replicationfactor": "3"
    },
    ...
},
```

#### Optional: Metadata Extractor 

As the metadata extractor is configured based on the event-type  properties \(`correlationId`/...\)and the harvester-api is also applying preconfigured defaults, you do not need to provide any mandatory configuration. 

But you can provide custom `appConfiguration` and `deploymentConfiguration` properties to replace the default configuration options.

Example for custom `appConfiguration` and `deploymentConfiguration`:

```text
"metadataExtractor": {
    ...
    "deploymentConfiguration": {
        "kubernetes.imagepullpolicy": "Always",
     ....
        "kubernetes.requests.memory": "512Mi",
    },
    "appConfiguration": {
        "spring.cloud.stream.bindings.input.consumer.concurrency": "6",
        ...
        "spring.cloud.stream.kafka.binder.replicationfactor": "3"
    },
    ...
},
```

### Create / Update Harvester Instance

First check if the harvester instance \( `name: ora-abc-harvester` \) already exists:

```text
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 https:/grnry-host/harvesters/instances/ora-abc-harvester
```

 if it's not found \(`404`\): create it with

```text
curl -X POST -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @harvester.json \
 https:/grnry-host/harvesters/instances
```

  or if it's already available `(200 SUCCESS)` update the entity with

```text
curl -X PUT -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @harvester.json \
  https:/grnry-host/harvesters/instances/ora-abc-harvester
```

The update may return a `200 (SUCCESS)` with the current definition of the harvester instance in case values were updated or a `304 (NOT MODIFIED)` without any content in case you update does not contain any changed values.

{% hint style="info" %}
It is safe to use a partial harvester definition in the update call. The api will only update/check provided/mandatory properties and will leave the rest unchanged. Therefore it is ok to do the update "blindly" without GET'ing the full harvester instance entity + updating this entity before using it during the update.
{% endhint %}

### Harvester State Management

On initial harvester instance creation the scdf stream is created, but not automatically started. The handling of the harvester state is similar to the persister state \(as both result in scdf streams being deployed\).

Check the current state of the harvester instance:

```text
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
  https:/grnry-host/harvesters/instances/ora-abc-harvester/state
```

Example response:

```text
{
    "status": "STOPPED",
    "_links": {
        "self": {
            "href": "https:/grnry-host/harvesters/instances/ora-abc-harvester/state"
        }
    }
}
```

| State | Description |
| :--- | :--- |
| UNKNOWN | status could not be determined |
| STOPPED | harvester instance is not deployed |
| DEPLOYING | harvester instance is being deployed |
| STOPPING | harvester instance is being stopped |
| RUNNING | harvester instance is running |
| RUNNING\_BUT\_OUTDATED | harvester instance is running with an outdated configuration, because it was updated since being started. For these changes to become effective the instance has to be stopped and started. |
| FAILED | harvester instance is deployed but not running because of other failures \(application startup, insufficient k8s resources, ...\) |

if stopped, start the harvester instance:

```text
curl -X POST -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -d @start.json \  
  https:/grnry-host/harvesters/instances/ora-abc-harvester/state
```

using

{% code title="start.json" %}
```text
{
  "action" : "START" 
}
```
{% endcode %}

Then wait and re-check until the persister is running. This might take some time depending on your settings regarding the liveliness probe intervals.

```text
curl -X POST -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
  https:/grnry-host/harvesters/instances/ora-abc-harvester/state
```

{% hint style="info" %}
There is no need to stop and start the harvester on each deployment. If your update api call for your harvester definition did not result in an actual update \(return code `304 NO UPDATE`\) the state of the harvester will still be `RUNNING`. Only changed configuration properties will result in a `RUNNING_BUT_OUTDATED` state and only then it's necessary to stop and start the harvester instance.
{% endhint %}

### Get Kubernetes Deployments of Harvesters and Persisters

In some scenarios, CI/CD pipelines need to modify a Harvester or Persister's Kubernetes deployment after the deployment via Harvester API.

This can be achieved by taking the `streamName` from a Harvester Instance or Persisters GET details API call and use the Kubernetes deployment label `spring-group-id` like so:

```text
kubectl get deployments -l spring-group-id=<streamName>
```

