---
description: This page lists known issues to the Granary platform.
---

# Known Issues

| Granary Component | Structured Name | Description | Version Occurred | Version Fixed | Ticket |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Profile Updater | GKI\_2019\_0001 | All profile updates fail after database connection failure. | 0.4.1 | - | [\#190](https://gitlab.alvary.io/grnry/scrum/issues/190) |
| Profile Updater | GKI\_2019\_0002 | \_delete operation deletes only latest grain value but no historic grain values. | 0.4.1 | - | [\#182](https://gitlab.alvary.io/grnry/scrum/issues/182) |
| Segment Table Creator | GKI\_2019\_0003 | Creating views on segments breaks the segment creation. | 0.4.2 | - | [\#220](https://gitlab.alvary.io/grnry/scrum/issues/220) |
| Segment Table Creator | GKI\_2019\_0004 | Failing segment creation causes resource contention. | 0.4.2 | - | [\#221](https://gitlab.alvary.io/grnry/scrum/issues/221) |
| Kafka Connect | GKI\_2019\_0005 | JDBC Connector creates more events than present in source table. | 0.4.1 | - | [\#198](https://gitlab.alvary.io/grnry/scrum/issues/198) [\#199](https://gitlab.alvary.io/grnry/scrum/issues/199) |
| Segment Store API | GKI\_2019\_0006 | Presto sends too many requests too Keycloak. | 0.4.3 | - | [\#193](https://gitlab.alvary.io/grnry/scrum/issues/193) |
| Profile Updater | GKI\_2019\_0007 | Profile Updater does not process json.dumps result. | 0.4.1 | - | [\#185](https://gitlab.alvary.io/grnry/scrum/issues/185) |
| Profile Updater | GKI\_2019\_0008 | Events with negative timestamp causes Profile Updater to crash loop. | 0.4.1 | - | [\#223](https://gitlab.alvary.io/grnry/scrum/issues/223) |

