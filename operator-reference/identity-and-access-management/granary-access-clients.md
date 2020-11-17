---
description: >-
  To Keycloak clients are applications that ask for certain privileges (roles) a
  user has to get access to resources. This page gives an overview of the
  clients and roles used by the granary platform.
---

# Granary Access Clients

## Clients

Currently granary uses the following keycloak clients

* scdf
* belt-api
* harvester-api
* profile-api
* event-api
* jdbc-api
* segment-management-api

## Administrative Clients

These clients provide access to the administration of the data flow infrastructure.

### scdf

Requests the user's access rights to operate on Spring Cloud Data Flow. This mainly includes creating and deploying harvesters.

| Role | Description |
| :--- | :--- |
| `modify` | for anything that involves mutating the state of the system |
| `destroy` | for anything that involves deleting streams, tasks etc. |
| `view` | for anything that relates to retrieving state |
| `schedule` | for scheduling related operation \(e.g. schedule a task execution\) |
| `manage` | for boot management endpoints |
| `deploy` | for deploying streams or launching tasks |
| `create` | for anything that involves creating, e.g. creating streams or tasks |

### belt-api

Requests the user's access rights to manage individual belts. These roles are defined by the belt author during belt definition and can be edited by anyone having the specified editor role. Each belt has a `viewer` attribute containing an array of all roles necessary for viewing it and also an `editor` attribute containing an array of all roles necessary for editing it. These values need to be added as roles in Keycloak and assigned to all authorized users. To create a new belt the user needs at least the default `belt_edit` role.

Example roles: `belt_view`, `belt_edit`, `belt_view_privileged`, `belt_edit_privileged`

### harvester-api

Requests the user's access rights to manage harvesters, i.e., creating event types or source types. Users need also access rights in scdf client.

Event types have two attributes, `editor`and `consumer`specifiying which users are allowed to edit and read the event type. If no `consumer`has been specified all users are allowed to read the event type, else only the ones matching the `consumer` role \(or matching the event type's `editor` role\). Editing the event type is only possible for users matching the provided `editor` role. If no `editor`role is set when creating the event type, the default `event_type_{type}_edit` is used. As the default for `type` is `data_in`, the default editor role is `event_type_data_in_edit`.

| Role | Description |
| :--- | :--- |
|  | user can read all event types that do not explicitly specify the `consumer` |
| `my_custom_role` | user can read all event types that specify no `consumer` or specify `consumer`as `my_custom_role`, can edit all event types that specify `editor` as `my_custom_role` |
| `event_type_data_in_edit` | Default role for a user to read and edit all `data_in` typed event types |
| `event_type_ttl_edit` | Default role for a user to read and edit all `ttl` typed event types |
| `source_type_read` | user can read all source types |
| `source_type_edit` | user can edit all source types |
| `harvester_read` | user can read all harvester instance |
| `harvester_edit` | user can edit all harvester instances |

### segment-management-api

Requests the user's access rights to manage segment jobs. These roles are defines by the segment job author during creation and can be edited by anyone having the specified `editor` role. Each segment job has a `viewer` attribute containing an array of all roles necessary for viewing it and also an `editor` attribute containing an array fo all roles necessary for editing it. These roles need to be added as roles in Keycloak and assigned to all authorized users. To create a new segment job the user needs at least the default `segment_job_edit` role.

Default roles: `segment_job_view`, `segment_job_edit`

## Data Out Clients

### profile-api \(a.k.a. Profile Store API\)

Requests a user's access rights to view certain profile grains in the profile store. If a user requests a profile, it will be build from all grains the user has access to. The required role for each grain is set by the belt writing it to the profilestore. Default value for reader column in the profilestore is `reader = _auth`. If a user has no roles, grains with `reader = _all` will still be accessible.

### event-api \(a.k.a. Event Store API\)

Requests user's access rights to view raw events. All role names in this client are combinations of event type name, event harvester name and an action-specifying suffix, e.i., `read`, separated by underscores.

Example role: `snowplow-a_snowplow-a-std-harvester_read` if the event type name is `snowplow-a` and the event harvester name is `snowplow-a-std-harvester`. 

### jdbc-api \(a.k.a. Segment Store API\)

#### Access PostgreSQL

Requests user's access rights to view certain parts of segments and enables technical users to access segment tables for OLAP queries. A jdbc-api role has the following structure  `<catalog>.<schema>.<table>.<column>`.

The following roles are required to view meta information

* `postgresql.information_schema.tables`
* `postgresql.information_schema.columns`
* `postgresql.information_schema.schemata`
* `system.jdbc.table_types`
* `system.jdbc.types`
* `system.jdbc.catalogs`
* `system.jdbc.tables`
* `system.jdbc.schemas`

The following role structure needs to be added for each segment the user needs to access:

`postgresql.segments.<segment name>`

Example role for viewing a segment: `postgresql.segments.profiles_seg1` if the segment table name is `profiles_seg1`.

#### Access Kafka Topics

Basically every Kafka Topic can be accessed when registered in Presto. To access Kafka Topics, the following role structure needs to be added:

`grnry-kafka.default.<TOPIC_NAME>`

Example role to view access Kafka messages of an Event Type's topic:   
`grnry-kafka.default.grnry_data_in_<EVENT_TYPE>`

or to view its DLQ \(dead letter queue\):  
`grnry-kafka.default.grnry_harvester_dlq_<EVENT_TYPE>`

For the **Event Viewer** you have to use the wildcard permission `grnry-kafka`.

{% hint style="warning" %}
Be aware that this will allow the User to view all Kafka Topics.
{% endhint %}

