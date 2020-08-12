---
description: Description of probe defaults accross Granary
---

# Kubernetes Liveness, Readiness Probes

By enabling Kubernetes liveness and readiness probes in Granary components, it is possible to automatically restart, e.g., deadlocked applications or Kafka-/database-related components that lost their connection to the respective persistence. Readiness probes are used to find out if a pod is ready to accept traffic. If a pod is not ready, it will be removed from the service's load balancer. Liveness probes are used to find out if a pod is running as expected. If a pod is not alive, it will be restarted by Kubernetes. Find more information on [Kubernetes probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/). 

### Spring Actuator Health Checks

Kubernetes provides a http probe that considers any return code of a GET request, that is greater than or equal to 200 and less than 400 a success. Any other code indicates failure. For Spring Applications there is a [Spring Actuator endpoint](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/production-ready-features.html#production-ready-endpoints) that allows the requester to check the application's [health](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/production-ready-features.html#production-ready-health) depending on multiple custom and build-in health conditions \(e.g., database connection\). It returns 200 if the application is healthy.

Below is a list of health indicators used in Granary, describing under which circumstances they report sucess/failures.

#### Datasource Health Indicator

The Spring Datasource Health Indicator is running the SQL-statement `SELECT 1;` against the given database connection. In case the query fails it will report service failure.

#### Kafka Health Indicator

The GRNRY Kafka Health Indicator checks three different scenarios against the connected Kafka brokers: 

1. it checks if it can open a producer and successfully send a message to a designated healthcheck topic,
2. it checks if the number of broker nodes is at least as high as the `offsets.topic.replication.factor` setting,
3. it checks if it can open a consumer and read messages from a designated healthcheck topic.

If any of the three checks fails the indicator will report service failure.

#### ThrottleControl Health Indicator

The GRNRY ThrottleControl Health Indicator is used in the Profile Updater if throtteling is active and checks that fetching the current lag is happening at the set interval. If the interval is exceeded it will report service failure

### Using cURL

Kubernetes also allows to perform a probe by executing a command in the target container. If the command succeeds, it should return exit code "0", so that Kubernetes considers the probe as successful. An example command could be `curl localhost:8080/actuator/health`. All Granary Docker components come with cURL installed.

### Component Healthcheck Configuration

| Component | Probing Active On |
| :--- | :--- |
| Belt API | Datasource Health Indicator |
| Belt Extractor | Prometheus Metrics Endpoint Availability |
| Eventstore API | Datasource Health Indicator |
| Harvester API | Datasource Health Indicator |
| Profilestore API | Datasource Health Indicator |
| Profile Updater | Datasource Health Indicator, Kafka Health Indicator, ThrottleControl Health Indicator |
| SCDF Apps | Datasource Health Indicator, Kafka Health Indicator |
| Segment Management API | Quarkus Internal \(always reports success\) |
| Snowplow Scala Stream Collector | Kafka Health Indicator \(producer-check only\) |

### Probe Defaults

| Property Name | Description | Default |
| :--- | :--- | :--- |
| `livenessProbe.periodSeconds` | Period for liveness probe in seconds  | `60` |
| `livenessProbe.timeoutSeconds` | Timeout liveness probe  | `10` |
| `livenessProbe.failureThreshold` | Number of unsuccessful liveness probes until pod is considered 'not alive'. | `10` |
| `livenessProbe.successThreshold` | Number of successful liveness probes until pod is considered 'alive'. | `1` |
| `readinessProbe.periodSeconds` | Period for readiness probe in seconds | `60` |
| `readinessProbe.timeoutSeconds` | Timeout readiness probe  | `10` |
| `readinessProbe.failureThreshold` | Number of unsuccessful readiness probes until pod is considered 'not ready'. | `10` |
| `readinessProbe.successThreshold` | Number of successful readiness probes until pod is considered 'ready'. | `1` |

{% hint style="info" %}
Please note that for SCDF source types the probe configuration needs to be set manually when registering the [Source Types](../installation/harvester-api/source-types.md#create-a-new-source-type-entity). To do this you need to add the following to the `deployer_config` column:

`"kubernetes.livenessProbeDelay": "30",  
"kubernetes.livenessProbePeriod": "60",  
"kubernetes.livenessProbeTimeout": "10",  
"kubernetes.readinessProbeDelay": "30",  
"kubernetes.readinessProbePeriod": "60",  
"kubernetes.readinessProbeTimeout": "10"`
{% endhint %}

