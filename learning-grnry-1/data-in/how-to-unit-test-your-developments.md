---
description: >-
  This page describes how you can test your developments for SCDF without the
  necessity to deploy it to a remote cluster, which is intended to save you a
  lot of time.
---

# How to test your developments

As a developer you want to make sure your applications run fast and without any disruptions. You want to make sure, there are no pitfalls within your code. Of course you could develop, transform the code to fit one single line and deploy an update of your code to the SCDF server. Then, the SCDF server deploys the updated code to the cluster and starts the pod. Afterwards, you need to log in to the pod, to see whether there are any errors. This is not convenient and takes a long time. Luckily, there is a better way to test your developments. In this chapter we show you how you can test your **Groovy** scriptable transform with GRNRY.

{% hint style="warning" %}
Currently, it is only possible to test Groovy scripts with the methods described here. Feel free to contribute wrappers for additional languages.
{% endhint %}

## Download the files

There is an open github repository [https://github.com/Alvary/scriptable-transform-test](https://github.com/Alvary/scriptable-transform-test) holding the files we need for testing. Clone this repository to your local desktop computer.

Afterwards, open a command line, navigate to the folder of scriptable-transform-test and type

```bash
gradlew assemble
```

This will build the application's jar file, which you may find then in the folder `build/libs`.

## Write your script

Write the first version of your Groovy script. You can get the payload via the variable payload, such as this one:

```groovy
def this_is_content = payload
```

At the end of your script, write:

```groovy
return my_transformed_payload
```

Note that you can pick any name you like for the variable, the content is the important part of it.

## Execute the script

Finally, you may run the script. Therefore execute:

```bash
java -jar <path_to_created_jar> --script=<path_to_script> --payload=<input_value>
```

Behind the `--script` parameter you define the file name of your script to be tested. The `--payload` parameter is a string representation of the data you would like to transform.

On the command line, you will then receive the following output. The interesting part starts at line 17, where we output the name of the file. Then in line 19/20 the payload is shown. In line 21/22, we inform you that your script is started and 23/24 comes from your application. In line 25/26 the resulting output is written, hence the content which you did return in your groovy script. The script tested is the hello.groovy script checked into the repository:

{% code title="Output of hello.groovy run with the application" %}
```bash
C:\Users\<user>\<path>\scriptable-transform-test>java -jar "build/libs/scriptable-transform-test-1.0-SNAPSHOT.jar" --script=hello.groovy --payload="Payload"

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.1.0.RELEASE)

Jul 17, 2019 2:29:05 PM org.springframework.boot.StartupInfoLogger logStarting
INFORMATION: Starting RunFileApp on \build\libs\scriptable-transform-test-1.0-SNAPSHOT.jar started by <user> in \scriptable-transform-test)
Jul 17, 2019 2:29:05 PM org.springframework.boot.SpringApplication logStartupProfileInfo
INFORMATION: No active profile set, falling back to default profiles: default
Jul 17, 2019 2:29:06 PM org.springframework.boot.StartupInfoLogger logStarted
INFORMATION: Started RunFileApp in 1.314 seconds (JVM running for 1.739)
Jul 17, 2019 2:29:06 PM io.grnry.scriptable.transform.test.RunFileApp run
INFORMATION: Value of script: hello.groovy
Jul 17, 2019 2:29:06 PM io.grnry.scriptable.transform.test.RunFileApp run
INFORMATION: Writing following payload to Groovy Script: Payload
Jul 17, 2019 2:29:06 PM io.grnry.scriptable.transform.test.RunFileApp run
INFORMATION: Starting Groovy script now!
Jul 17, 2019 2:29:08 PM java_util_logging_Logger$info call
INFORMATION: Here I am executing Groovy
Jul 17, 2019 2:29:08 PM io.grnry.scriptable.transform.test.RunFileApp run
INFORMATION: Groovy script returned: Payload has been modified
```
{% endcode %}





