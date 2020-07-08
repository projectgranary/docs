---
description: Description of probe defaults accross Granary
---

# Kubernetes Liveness, Readiness Probes

By enabling Kubernetes liveness and readiness probes in Granary components, it is possible to automatically restart, e.g., deadlocked applications or Kafka-/database-related components that lost their connection to the respective persistence. Find more information on [Kubernetes probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/).

### Spring Actuator Health Checks

Kubernetes provides a http probe that considers any return code of a GET request, that is greater than or equal to 200 and less than 400 a success. Any other code indicates failure. For Spring Applications there is a [Spring Actuator endpoint](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/production-ready-features.html#production-ready-endpoints) that allows the requester to check the application's [health](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/production-ready-features.html#production-ready-health) depending on multiple custom and build-in health conditions \(e.g., database connection\). It returns 200 if the application is healthy.

The following Granary components expose their health status at the `/actuator/health` endpoint:

* Belt API
* Harvester API
* Profilestore API
* Eventstore API
* Kafka Profile Update

### Using cURL

Kubernetes also allows to perform a probe by executing a command in the target container. If the command succeeds, it should return 0, and the kubelet considers the probe as succeeded. An example command could be `curl localhost:8080/actuator/health`. All Granary Docker components come with cURL installed.

### Probe Defaults

| Property Name | Description | Default |
| :--- | :--- | :--- |
| livenessProbe.httpGet.path | Liveness probe GET path. | `/actuator/health` |
| livenessProbe.httpGet.port | Liveness probe port. | `8080` |
| livenessProbe.periodSeconds | Period for liveness probe in seconds  | `10` |
| livenessProbe.timeoutSeconds | Timeout liveness probe  | `5` |
| livenessProbe.failureThreshold | Number of unsuccessful liveness probes until pod is considers 'not alive'. | `10` |
| readiessProbe.httpGet.path | Readiness probe GET path. | `/actuator/health` |
| readiessProbe.httpGet.port | Readiness probe port. | `8080` |
| readinessProbeperiodSeconds | Period for readiness probe in seconds | `10` |
| readinessProbe.timeoutSeconds | Timeout readiness probe  | `5` |
| readinessProbe.failurethreshold | Number of unsuccessful readiness probes until pod is considered 'not ready'. | `5`, exception Kafka Profile Update `3` |



