---
description: This page contains instructions how to create a Project in Granary.
---

# Create Project in Granary

## What is a project in Granary?

Projects in Granary enforce a multi-tenancy view on Granary objects (Event Types, Harvesters and Belts) as well as the data. Each Granary object needs to be created in the context of a project.&#x20;

By default, users have read access to the `global` project. See [migration guide](../operator-reference/migration-guide/projects-use-case-migration-concept.md) for more details on the `global` project. In order to create Granary objects themselves, users need to have write access to at least one project. The write access can be granted by the so-called "data owner" of a project. But before the user assignment can be done, let us create your first Granary project!

{% hint style="info" %}
Learn more about the technical realization of Projects in the [developer reference](../developer-reference/projects.md).
{% endhint %}

## Create a Project in Granay

In this chapter we are going to learn the steps to create a project in Granary and which permissions need to be applied to the user.

{% hint style="warning" %}
In Granary 1.2 it is only possible to create Projects using the API endpoints described below. There is not UI available for it.
{% endhint %}

First, the project-creating user needs to have `project_creator` Keycloak role in client `project-api` to call the [Project API](../developer-reference/api-reference/project-api.md#create-project)'s CREATE method.&#x20;

To achieve this, you can find the steps needed in [Add Roles to Keycloak Users guide](../operator-reference/identity-and-access-management/adding-roles-to-keycloak-clients.md#assigning-a-role-to-a-user) or open a ticket with Granary support (recommended) to get the role assigned. If following the guide, pay attention to&#x20;

* select the value `project-api` from the "Client Roles" drop down and
* add the `project_creator` role from "Available Roles" to "Assigned Roles".

{% hint style="info" %}
Consult the [Granary Access Clients](../operator-reference/identity-and-access-management/granary-access-clients.md#project-api) documentation on "project-api" for more details.
{% endhint %}

Now the user is ready to make use of the [Project APIs](../developer-reference/api-reference/project-api.md) endpoints with the [Postman Collection](../developer-reference/api-reference/#postman-collection) to create the project.

Second, we create the project with the [SQLPad](https://getsqlpad.com/#/) deploy option so that we can access our data with Granary's built-in SQL console using SQL. Using this option, SQLPad instance will be deployed and connected to corresponding project schema.

[POST /projects?deploy=sqlpad](../developer-reference/api-reference/project-api.md#create-project)

```json
{
  "name": "projecttotestlivesegment",
  "displayName": "projecttotestlivesegment",
  "description": "Test Granary Project for live segement description"
}
```

Response will be:

```json
{
    "name": "projecttotestliveseg",
    "displayName": "ProjecttotestLivesegment",
    "description": "Test Granary Project for live segement description",
    "_links": {
        "self": {
            "href": "https://development.internal.analytics.cc.syncier.cloud/projects/projecttotestliveseg?expand=sqlpad"
        }
    }
}
```

Third, the project gets created and we can see it in Granary UI:

![](<../.gitbook/assets/Screenshot 2021-11-09 at 14.24.19.png>)

{% hint style="info" %}
In case if we forget to create a project without `?deploy=sqlpad` parameter, one can also deploy [SQLPad via Helm](../operator-reference/installation/with-helm/sqlpad.md) using the project's database and Keycloak credentials. We can confirm SQLPad UI is running for the schema project by running the [GET project request](../developer-reference/api-reference/project-api.md#get-project-details) with `?expand=sqlpad`.
{% endhint %}



## Assign User to a Project

The project's `data_owner` is the role which can have following permissions:&#x20;

* Contact point for this project, can grant access for event type sharing requests.
* Can add or remove users to the project and manage roles within the project.

User can use the [Project APIs](../developer-reference/api-reference/project-api.md) to get the access to users assigned to the specified project

We can follow the steps mentioned below:&#x20;

#### First to search for users to get keycloak ID, we can use Project API with the following request:

[GET /users?search=\<user's name>](../developer-reference/api-reference/project-api.md#get-users-to-add-them-to-project)

This returns an array of users available in Granary to be assigned, if we have not specified any user in the `search` parameter. Requires role `user_viewer` in `project-api` client.

For example, I searched for `user=ma`

[GET /users?search=ma](../developer-reference/api-reference/project-api.md#get-users-to-add-them-to-project)

Then the response will be:&#x20;

```json
[
    {
        "id": "21e3949a-b48e-45b7-bc0b-ba682b83928c",
        "userName": "madhuri.mozar@grnry.io",
        "email": "madhuri.mozar@grnry.io",
        "lastName": "Mozar",
        "firstName": "Madhuri"
    },
    {
        "id": "1ec0fe45-a542-4a27-b7a5-10046775a9bc",
        "userName": "simon.maling@grnry.io",
        "email": "simon.maling@grnry.io",
        "lastName": "Maling",
        "firstName": "Simon"
    }
]


```

#### The next step is to add project role to Keycloak user ID using following request:&#x20;

[POST /projects/:project-name/users/:user-id](../developer-reference/api-reference/project-api.md#add-user-to-project)

Where `:project-name` needs to be provided as input to the request and the `:user-id` is the keycloak user identifier which received in first step.

With this call, the user is added to the specified project with roles set in request body. It requires the role `data_owner` in the particular project.

The request's body is looks like:&#x20;

```json
{
  "roles": [
    "editor"
  ]
}
```

It returns empty response with status code `200 OK` for successfully assigned the role.&#x20;

#### Third step to verify the project roles of Keycloak is assigned to user using the following request:

[GET /projects/:project-name/users](../developer-reference/api-reference/project-api.md#get-members-of-the-project)

Where `:project-name` needs to be provided as input to the request.

Which returns an array of users assigned to the specified project and their roles in the project.&#x20;

```json
[
    {
        "id": "21e3949a-b48e-45b7-bc0b-ba682b83928c",
        "userName": "madhuri.mozar@grnry.io",
        "email": "madhuri.mozar@grnry.io",
        "lastName": "Mozar",
        "firstName": "Madhuri",
        "roles": [
            "data_owner",
            "editor"
        ]
    }
]
```

Also, we can see the project is now accessible in the Granary UI for the user:

![](<../.gitbook/assets/Screenshot 2021-11-25 at 16.12.01.png>)

#### Also, we can update the roles using following request:&#x20;

[PUT /projects/:project-name/users/:user-id](../developer-reference/api-reference/project-api.md#update-user-roles-in-the-project)

Where `:project-name` needs to be provided as input to the request and the `:user-id` is the keycloak user identifier which received in first step.

It adds the user to the specified project with roles set in request body. Requires the role `data_owner`

The request looks like:

```json
{
  "roles": [
    "viewer"
  ]
}
```

It returns empty response with status code `200 OK` for successfully updated the role.&#x20;

By setting an empty array, we can also revoke a user's access to the project.
