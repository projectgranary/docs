---
description: This page contains the technical architecture of Projects in Granary
---

# Projects

## Overview of a Project

Granary Projects are represented as [Keycloak clients](https://www.keycloak.org/docs/latest/server\_admin/index.html#\_clients). Keycloak client was chosen as the main entity to represent a project in Granary because it is the natural carrier of the roles which are needed for access restriction. Such approach allows ops engineers to operate with well known Keycloak based access management and effectively implement resource isolation in Granary&#x20;

An example of the project (Keycloak client) entry in [JWT token](projects.md#overview-of-a-project):

<img src="https://lh3.googleusercontent.com/eM7ljg0ges9EcPqd0p8NVekVtEZGF1h7LaogOjKL4AoKQO0pJ-N_xdI8j32JqRVFaffuIuDm0YbUE8X9Oqo7hVBzw6C8fO5Fvix6bBsvUQ8usWlIei6VM0ItUZmr8GF6Og" alt="" data-size="original">

When such JWT is received by a component's endpoint such as Belt API by URL&#x20;

`GET /project/my_project/belts/1`

Spring framework with customized Spring Keycloak security may recognize from the URL path that access is requested to the `my_project` project components so it will try to find corresponding resource access entry in the JWT and load roles to the security context. \


Every single project may have three roles:&#x20;

* `data_owner` - user with this role is able to add/remove other users to the project
* `editor` - allows to edit granary components such as belts, harvesters, eventypes
* `viewer` - allows to read granary components such as belts, harvesters, eventypes

{% hint style="info" %}
Consult the [Granary Access Clients](../operator-reference/identity-and-access-management/granary-access-clients.md#project-api) documentation for more details.
{% endhint %}

## Architecture of Projects <a href="#docs-internal-guid-06a5e216-7fff-927b-e181-d2437d40037b" id="docs-internal-guid-06a5e216-7fff-927b-e181-d2437d40037b"></a>

\
The `grnry-management-api` is a service where [Project API endpoints](api-reference/project-api.md) are hosted. The service is responsible for next actions:&#x20;

* project management
* search users in keycloak
* add/remove users to a project.

The `grnry-management-api` is called to create/read projects. It operates with three other Granary services:&#x20;

![](https://lh5.googleusercontent.com/\_zcQ1IZpH6mFCI0BrAPFLBqYWXT\_woDv8SWJ12RSf4DVbtBVzL7cMQJjj4ruxYm4OMbJqSKpKLc2iTQss8GeJ9M7DqFYm\_CFzReFfc4VSl73YG5Z1kAy0w0H8Bq2w\_LASg)

\


For a better understanding of how projects are organized, let's review the process of project creation and how `grnry-management-api` interacts with Keycloak, PostgreSQL, and Kubernetes.&#x20;

![Create project sequence diagram](https://lh3.googleusercontent.com/UXxXd2xS4UYAOs04lKrMI7pnVUCWG9QPgmLT49zk9Ikj08YpsfeBfIxzMepfmj7-49R7BcZa5kHeTTHlVsjDeSQnr4i4QNIiai6cJ8pk6HrdAu1NcnHQTD9ZEp4tpvqDzA)

1. Create project rest call (POST /projects) with valid JWT with `project_creator` role from `project-api` Keycloak client resource.
2. Create entities in Keycloak:
   1. Create a client in Keycloak. Client will be created with attributes (client\_type, display\_name, description)
   2. Add new roles to the client (`data_owner`, `viewer`, `editor`)
   3. Assign `data_owner` role to the current user&#x20;
3. Create schema and technical user in PostgreSQL.
4. Create Kubernetes secret with access credentials to PostgreSQL schema (technical user credentials).



\
