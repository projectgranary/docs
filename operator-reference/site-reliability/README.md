---
description: >-
  Actions and Procedures to avoid and (if happened) handle Granary Component
  Disasters
---

# Site Reliability

Content shall mostly come from [https://gitlab.alvary.io/grnry/scrum/issues/118](https://gitlab.alvary.io/grnry/scrum/issues/118) 

This guide proposes actions and procedures to avoid and \(if happened\) handle Granary component disasters. It discusses Keycloak \(the IAM tool\), Kafka \(message queue\), Postgres/Citus \(event and profile storage\) as well as Belt development.

Some of these actions are part of the standard Granary setup, mainly implemented in helm charts. Others need manual effort by the Granary Platform Administrator.

Be aware of that reliable infrastructure cannot avoid human error. Therefore, make sure all component’s data additionally resides in backup storage.

