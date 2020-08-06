---
description: >-
  Actions and Procedures to avoid and (if happened) handle Granary Component
  Disasters
---

# Site Reliability

These guides propose actions and procedures to avoid and \(if happened\) handle Granary component disasters. It discusses Keycloak \(the IAM tool\), Kafka \(message queue\), Postgres/Citus \(event and profile storage\), Kubernetes Probes, Logging as well as Belt development.

Some of these actions are part of the standard Granary setup, mainly implemented in Helm Charts. Others need manual effort by the Granary Platform Administrator.

Be aware of that reliable infrastructure cannot avoid human error. Therefore, make sure all component’s data additionally resides in backup storage.

