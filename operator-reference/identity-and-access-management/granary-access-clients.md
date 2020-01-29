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
* profile-api
* event-api
* jdbc-api

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

Requests the user's access rights to manage individual belts. These roles are defined by the belt author during belt definition and can be edited by anyone having the specified editor role. Each belt has a `viewer` attribute containing an array of all roles necessary for viewing it and also an `editor` attribute containing an array of all roles necessary for editing it. These values need to be added as roles in Keycloak and assigned to all authorized users.

Example roles: `belt_view`, `belt_edit`, `belt_view_privileged`, `belt_edit_privileged`

## Data Out Clients

### profile-api \(a.k.a. Profile Store API\)

Requests a user's access rights to view certain profile grains in the profile store. If a user requests a profile, it will be build from all grains the user has access to. The required role for each grain is set by the belt writing it to the profilestore. Default value for reader column in the profilestore is `reader = _auth`. If a user has no roles, grains with `reader = _all` will still be accessible.

### event-api \(a.k.a. Event Store API\)

Requests user's access rights to view raw events. All role names in this client are combinations of event type name, event harvester name and an action-specifying suffix, e.i., `read`, separated by underscores.

Example role: `snowplow-a_snowplow-a-std-harvester_read` if the event type name is `snowplow-a` and the event harvester name is `snowplow-a-std-harvester`. 

### jdbc-api \(a.k.a. Segment Store API\)

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

The following role structure needs to be added for each segment the user needs to access.

`postgresql.segments.<segment name>`

Example role for viewing a segment: `postgresql.segments.profiles_seg1` if the segment table name is `profiles_seg1`.



