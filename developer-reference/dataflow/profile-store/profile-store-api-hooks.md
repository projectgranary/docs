---
description: This page describes how Profile Store API hooks are developed and deployed.
---

# Profile Store API Hooks

## Implementing Profile Store API Hooks

The Profile Store API allows implementation of custom Hooks to intercept \(and possibly mutate\) outgoing profiles to e.g. ensure that critical data is protected depending on use case requirements.

The Profile Store API hook runtime can have multiple Hooks. Each one will be executed on the fetched profiles in the order the Hooks are loaded by JVM. This is achieved by using the [Spring Component Scan Annotation](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/context/annotation/ComponentScan.html), it allows to load a Java Bean from the classpath into the ApplicationContext.

Exceptions caused by Hook methods will be propagated further and cause an API response with status code HTTP500.

Each Hook needs to be implemented as a [Spring Boot Service.](https://www.tutorialspoint.com/spring_boot/spring_boot_service_components.htm)

### **Hook Interface**

Allows the implementation of Hooks which will be executed before the HTTP Response is send to the caller.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Method</th>
      <th style="text-align:left">Definition</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">executeBeforeSend</td>
      <td style="text-align:left">
        <p>Intercepts profile and allows mutation before sending to API caller.</p>
        <p></p>
        <p><b>Parameters:</b>
        </p>
        <p><code>String profileType:</code>Profile Type of the profile.</p>
        <p><code>String correlationId:</code>Correlation ID of the profile.</p>
        <p><code>List&lt;String&gt; roles:</code>Roles the requesting user holds.</p>
        <p><code>JsonNode profile:</code>The profile fetched from Profile Store.</p>
        <p><b>Returns:</b>
        </p>
        <p><em> </em><code>JsonNode:</code>Possibly mutated profile.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">shouldExecuteBeforeSend</td>
      <td style="text-align:left">
        <p>Allows the user to implement a method to validate if</p>
        <p><a href="../../../../io/grnry/profilestore/hooks/Hook.html#executeBeforeSend-java.lang.String-java.lang.String-java.util.Collection-JsonNode-"><code>executeBeforeSend(java.lang.String, java.lang.String, java.util.Collection&lt;java.lang.String&gt;, JsonNode)</code></a> 
        </p>
        <p>is called.</p>
        <p></p>
        <p><b>Parameters:</b>
        </p>
        <p><code>String profileType:</code>Profile Type of the profile.</p>
        <p><code>String correlationId:</code>Correlation ID of the profile.</p>
        <p><code>List&lt;String&gt; roles:</code>Roles the requesting user holds.</p>
        <p><code>JsonNode profile:</code>The profile fetched from Profile Store.</p>
        <p><b>Returns:</b>
        </p>
        <p><code>Boolean: </code>If executeBeforeSend should be called.</p>
      </td>
    </tr>
  </tbody>
</table>### **Example Hook Implementation**

{% code-tabs %}
{% code-tabs-item title="DemoHook.java" %}
```java
package io.grnry.profilestore.hooks;

import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.node.ObjectNode;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Service;

import java.util.Collection;

@Service
@Qualifier("demoHook")
public class Demo implements Hook {

    public JsonNode executeBeforeSend(String profileType, String correlationId, Collection<String> roles, JsonNode profile) {
        // Adjust the profile as needed
        return profile;
    }

    public Boolean shouldExecuteBeforeSend(String profileType, String correlationId, Collection<String> roles, JsonNode profile) {
        // Validate if executeBeforeSend should be called
        return true;
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
**Note:** Ensure that the hook is in the package **io.grnry.profilestore.hooks**. Otherwise the Hook Loader will not find the custom Hook.
{% endhint %}

## **Building a custom Hook**

Ensure that the Hook is compiled as a fatJar containing its needed dependencies. 

{% code-tabs %}
{% code-tabs-item title="build.gradle" %}
```groovy
dependencies {
       compile project(':hooks-api')
       compile('com.fasterxml.jackson.core:jackson-databind')
       compileOnly('org.springframework.boot:spring-boot-starter-web') 
   }
   
   bootJar {
       enabled = false // disabled as this Hook has no main class
   }
   
   jar {
       enabled = true
       from {
           configurations.compile.collect { it.isDirectory() ? it : zipTree(it) }
       }
   }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Deploying a custom Hook

In order to a custom Hook, the Hook fatJar needs to be placed inside the hooks folder `/hooks` used by the Profile Store API's [Dockerfile](https://gitlab.alvary.io/grnry/profilestore-api/blob/master/Dockerfile#L22). A restart of component Profile Store API service with the updated docker image is necessary to use a Hook.

