---
description: 'On this page, you get the technical details about our scriptable transform.'
---

# Scriptable Transform

{% hint style="info" %}
Refer to the [Granary Data-In Best Practices](../../../learning-grnry-1/data-in/best-practices-1/) to learn how to transform a source type's input to JSON format that is required by an Event Type.
{% endhint %}

The scriptable transform is a microservice, which should transform incoming data to your desired structure in JSON. You may do any kind of preprocessing here. This microservice receives coding in order to execute the transformation of your choice. It is based on the Scriptable Transform Processor of Spring and can take Groovy, JavaScript, Ruby or Python \(for Java\) as languages.

The scriptable transform typically is the place where you need to write the most information and do mappings etc. This is the main enhancement point for you during the data-in pipeline. You are going to write a script in one of the defined languages for this. In this chapter: [How to test your developments](../../../learning-grnry-1/data-in/best-practices-1/how-to-unit-test-your-developments.md) we are showing you, how to write unit tests, which help you in writing your applications without having to deploy them to a cluster the whole time. 

In order to process the data within your script, there is the global variable `payload`. This variable holds the data you are getting from the outside and can be used within your function. Please note, that it is not possible to use more than one script, however you might define functions within your script, to which you refer.

In order to get data out of the script, you just return the message/event you want to process in subsequent steps.

Within the script you can do everything you like, such as lookups of configs defined in the file, parameter conjunctions, conditions and loops as long as it fits into one file. It is not possible to define more than one file. In addition it is necessary to convert the file content into a string for deployment, for example by doing as described in the [Easing development](../../../learning-grnry-1/data-in/best-practices-1/easing-development.md) section.

**Name**: grnry-scriptable

**Parameters**: See [shared parameters](grnry-components-and-parameters.md).

{% hint style="info" %}
Currently, there are no GRNRY-specific parameters for JDBC. The benefit of this source type is encryption of the processed messages.
{% endhint %}

**Source Parameters:**

The appropriate parameters for configuration can be found here:

{% embed url="https://github.com/spring-cloud-stream-app-starters/scriptable-transform/blob/master/spring-cloud-starter-stream-processor-scriptable-transform/README.adoc" caption="Scriptable Transform Parameters" %}

A sample for a scriptable processor might look like this: 

```yaml
"transform": {
        "app": "grnry-scriptable",
        "version": "latest",
        "deploymentConfiguration": {
            "kubernetes.readinessprobedelay": "120",
            "kubernetes.volumes": "[{name: 'secret', secret: { secretName : 'grnry-base-encryption-token' , defaultMode : '256' }}, {name: 'db-secret', secret: { secretName : 'grnry-pg-citus-secret' , defaultMode : '256' }}]",
            "kubernetes.limits.memory": "512Mi",
            "kubernetes.limits.cpu": "500m"
            "kubernetes.volumemounts": "[{name: 'secret', mountPath: '/usr/src/app/rsa_privatekey.key' , subPath: 'rsa_privatekey.key' , readOnly : 'true' },{name: 'secret', mountPath: '/usr/src/app/rsa_publickey.key' , subPath: 'rsa_publickey.key' , readOnly : 'true' }, {name: 'db-secret', mountPath: '/usr/src/app/db-secret' , readOnly : 'true' }]",
            "kubernetes.requests.cpu": "500m",
            "kubernetes.requests.memory": "512Mi",
            "kubernetes.imagepullpolicy": "Always",
            "kubernetes.livenessprobedelay": "120"
        },
        "appConfiguration": {
            "spring.cloud.stream.kafka.binder.autoCreateTopics" : true,
            "spring.cloud.stream.bindings.input.consumer.concurrency": 3,
            "spring.cloud.stream.bindings.input.consumer.partitioned": true,
            "spring.cloud.stream.bindings.output.producer.partitionCount": 3,
            "spring.cloud.stream.bindings.output.producer.autoAddPartitions": true,
            "spring.cloud.stream.kafka.bindings.input.consumer.resetOffsets": true,
            "spring.cloud.stream.kafka.bindings.input.consumer.startOffset": "earliest",
            "management.endpoints.web.exposure.include": "prometheus,info,health",
            "management.metrics.export.prometheus.enabled": true
        },
        "language": "groovy",
        "script": "import groovy.json.JsonOutput\\nclass Vertrag {\\n    String kd_nr\\n    String v_nr\\n    String text\\n}\\ndef splittedRow = (new String(payload)).split(',')\\ndef c = new Vertrag()\\nc.kd_nr = splittedRow[0]\\nc.v_nr = splittedRow[1]\\nc.text = splittedRow[2]\\nreturn JsonOutput.toJson(c)"
    }
```

You should note, that the script with your code is given in the `script` parameter. The data which is sent to the script, is available in the script via the parameter `payload`, which you can access in your development.

