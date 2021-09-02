---
description: >-
  To Keycloak clients are applications that ask for certain privileges (roles) a
  user has to get access to resources. This page gives an overview of the
  clients and roles used by the granary platform.
---

# Granary Access Clients

## Clients

Currently Granary uses the following keycloak clients:

#### Administrative Clients

* scdf
* project-api
* Projects' clients = {_x_ \| _x_ is name of a project}

#### Data Out clients

* profile-api
* event-api
* jdbc-api

## Administrative Clients

These clients provide access to the control plane of the data flow infrastructure.

### scdf

Requests the user's access rights to operate on Spring Cloud Data Flow \(SCDF\). This mainly includes creating and deploying harvesters. Usually it is sufficient to grant roles in the scdf client to the technical user that the Harvester API uses to interact with keycloak. This user needs all of the below mentioned roles.

| Role | Description |
| :--- | :--- |
| `modify` | for anything that involves mutating the state of the system |
| `destroy` | for anything that involves deleting streams, tasks etc. |
| `view` | for anything that relates to retrieving state |
| `schedule` | for scheduling related operation \(e.g. schedule a task execution\) |
| `manage` | for boot management endpoints |
| `deploy` | for deploying streams or launching tasks |
| `create` | for anything that involves creating, e.g. creating streams or tasks |

### project-api

Defines the user's access rights to 

* create new projects \(not to manage them afterwards\): `project_creator`
* retrieve user list \(not to assign / remove user to projects afterwards\): `user_viewer`

### Projects' clients

For each project there is a separate client that is automatically created by [Project API](../../developer-reference/api-reference/project-api.md#create-project). The client name is equal to the project name. The assignment of the client's roles to users defines their rights to manage Granary objects like Belts, Harvesters, Event Types, Persisters or Source Types in the scope of that project.

Each project client offers three roles `editor`, `viewer` and `data_owner`. Every user having one of these roles is considered a member of that project. Every new project member will be assigned the project client's `viewer` role.

| Role | Description |
| :--- | :--- |
| `editor` | manage Granary objects: create, update, delete, change status |
| `viewer` | view Granary objects and their status |
| `data_owner` | add new members to the project, manage data access |

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

