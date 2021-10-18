---
description: Microservice to create Projects in Granary.
---

# Project API

## Paths

* POST /projects
* GET /projects/{project-name}
* PUT /projects/{project-name}
* GET /projects/{project-name}/users
* GET /projects
* GET /users
* POST /projects/{project-name}/users/{user-id}
* PUT /projects/{project-name}/users/{user-id}

{% swagger baseUrl="https://api.grnry.io" path="/projects" method="post" summary="Create Project" %}
{% swagger-description %}
Creates a project. A technical name for the project entity will be derived from given 

`displayName`

 by removing all special characters, replacing white spaces with hyphens and limiting the length to 20 characters. Should the created project name be already in use, the last four characters will be replaced by a suffix of numbers.

\


Granary projects are represented as Keycloak client with specific attributes. On 

_Create Project API call  _

the following will be created:

\


    • Keycloak client entry with attributes (client_type, description, display_name)

\


    • Database schema    

\


    • Database user, created user will be set as owner of created schema

\


    • Kubernetes secret where database user credentials are stored 

\




\


To be able to create a project, the 

`project_creator`

 role is required in client 

`project-api`

. Otherwise, 

_forbidden_

 error code will be returned
{% endswagger-description %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="deploy" type="string" %}
Coma separated list of deploy options. 

\


Supported options: 

\


\- sqlpad: SqlPad instance will be deployed and connected to corresponding project schema

\




\


Example: 

\


/projects?deploy=sqlpad

\



{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="object" %}
Project description
{% endswagger-parameter %}

{% swagger-parameter in="body" name="displayName" type="object" %}
Human readable name. A technical name will be derived from it.
{% endswagger-parameter %}

{% swagger-response status="200" description="Cake successfully retrieved." %}
```
{
  "name": "demo_granary_project",
  "displayName": "Demo Granary Project",
  "description": "Demo Granary Project description"
}
```
{% endswagger-response %}

{% swagger-response status="400" description="Project display name is not valid. As an example when 'pg' chars are not allowed at the begging" %}
```
{
  "timestamp": "2021-09-01T14:56:25.365+00:00",
  "message": "Body parameter 'displayName' could not be used to create a valid Project name. Please try another displayName that is not 'PG project'.",
  "type": "bad_parameter_value",
  "invalidParams": [
    {
      "name": "displayName",
      "reason": "could not be used to create a valid Project name. Please try another displayName that is not 'PG project'"
    }
  ],
  "details": "uri=/projects"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="When token doesn't have project-api client access with project-creator role." %}
```
{
  "timestamp": "2021-08-31T13:10:29.446+00:00",
  "message": "Access forbidden due to missing roles.",
  "type": "entity_not_accessible",
  "details": "uri=/projects"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/:project-name" method="get" summary="Get Project details" %}
{% swagger-description %}
Returns all details of a given project. 

\




\


Requires the role: any of 

`data_owner`

 , 

`viewer`

,  

`editor`
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Project name.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="expand" type="string" %}
Expand options. Response will be enhanced with extra data.

\


Example: 

\


?expand=sqlpad 
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
  "name": "demo_granary_project",
  "displayName": "Demo Granary Project",
  "description": "Demo Granary Project description",
  "dataAccess": {
    "host": "grnry-pg",
    "port": "5432",
    "ssl": "true",
    "sslValidate": "false",
    "database": "postgres",
    "schema": "demo_granary_project",
    "secretName": "demo-granary-project-pg-credentials",
    "secretKeyUser": "postgresql-username",
    "secretKeyPassword": "postgresql-password",
    "readReplicaHost": null,
    "readReplicaPort": null
  },
  "sqlpad": {
    "_links": {
      "self": {
        "href": "https://development.internal.analytics.cc.syncier.cloud/sqlpad/demo_granary_project/"
      }
    }
  },
  "_links": {
    "self": {
      "href": "https://development.internal.analytics.cc.syncier.cloud/projects/demo_granary_project?expand=sqlpad"
    }
  }
}
```
{% endswagger-response %}

{% swagger-response status="403" description="When roles cannot be loaded for the specific project. Also applicable to the case when project is not found because of roles missing." %}
```
{
  "timestamp": "2021-09-01T15:01:32.873+00:00",
  "message": "Access forbidden due to missing roles.",
  "type": "entity_not_accessible",
  "details": "uri=/projects/demo"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/:project-name" method="put" summary="Update Project" %}
{% swagger-description %}
Updates a project. All body parameters are optional. Empty fields will be set to "", missing fields will remain unchanged. 

\




\


Requires the role 

`data_owner`

.
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Project name.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="displayName" type="string" %}
Project display name.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="description" type="string" %}
Project description.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
{
  "name": "demo_granary_project",
  "displayName": "Demo Granary Project",
  "description": "new description"
}
```
{% endswagger-response %}

{% swagger-response status="403" description="When roles cannot be loaded for the specific project. Also applicable to the case when project is not found because of roles missing." %}
```
{
  "timestamp": "2021-08-31T13:10:29.446+00:00",
  "message": "Access forbidden due to missing roles.",
  "type": "entity_not_accessible",
  "details": "uri=/projects/{projectName}"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/:project-name/users" method="get" summary="Get Members of the Project" %}
{% swagger-description %}
Returns an array of users assigned to the specified project. 

\


User details contains: 
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Project name.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
[
  {
    "id": "860867d1-f5a9-4737-9f60-4783a4c52191",
    "userName": "userName",
    "email": "user@email.com",
    "lastName": "Firstname",
    "firstName": "Lastname",
    "roles": [
      "data_owner",
      "editor"
    ]
  }
]
```
{% endswagger-response %}

{% swagger-response status="403" description="" %}
```
{
  "timestamp": "2021-08-31T13:10:29.446+00:00",
  "message": "Access forbidden due to missing roles.",
  "type": "entity_not_accessible",
  "details": "uri=/projects/{projectName}/users"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects" method="get" summary="Get a User's Projects" %}
{% swagger-description %}
Returns an array of projects available for currently authenticated user.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
[
  {
    "name": "demo_0000",
    "displayName": "Demo",
    "description": "Demo Granary Project description",
    "links": [
      {
        "rel": "self",
        "href": "https://development.internal.analytics.cc.syncier.cloud/projects/demo_0000?expand="
      }
    ]
  },
  {
    "name": "data_project",
    "displayName": "Data Project",
    "description": "Data Project description",
    "links": [
      {
        "rel": "self",
        "href": "https://development.internal.analytics.cc.syncier.cloud/projects/data_project?expand="
      }
    ]
  }
]
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/users" method="get" summary="Get Users to Add them to Project" %}
{% swagger-description %}
Returns an array of users available in Granary to be assigned.

\


Requires role 

`user_viewer`

 in 

`project-api`

 client.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="search" type="string" %}
Search string for users.
{% endswagger-parameter %}

{% swagger-response status="200" description="for search=jo" %}
```
[
  {
    "id": "6dbedea6-3b29-4d60-af83-b9105640cb82",
    "userName": "joe.doe@helloworld.io",
    "email": "joe.doe@helloworld.io",
    "lastName": "Doe",
    "firstName": "Joe"
  }
]
```
{% endswagger-response %}

{% swagger-response status="403" description="When token doesn't have the role user_viewer in client project-api." %}
```
{
  "timestamp": "2021-08-31T13:10:29.446+00:00",
  "message": "Access forbidden due to missing roles.",
  "type": "entity_not_accessible",
  "details": "uri=/users"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/:project-name/users/:user-id" method="post" summary="Add User to Project" %}
{% swagger-description %}
Adds the user to the specified project with roles set in request body.

\




\


Requires the role 

`data_owner`
{% endswagger-description %}

{% swagger-parameter in="path" name="user-id" type="string" %}
UUID like keycloak user identifier.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Project name.
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="roles" type="array" %}
An array of roles to be assigned for specified user-id
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
<Response body is empty>
```
{% endswagger-response %}

{% swagger-response status="400" description="When the passed role is not in the list of available roles" %}
```
{
  "timestamp": "2021-09-01T14:46:42.116+00:00",
  "message": "Passed object is not readalbe JSON parse error: Cannot deserialize value of type `io.grnry.project.services.ProjectRole` from String \"editofr\": not one of the values accepted for Enum class: [viewer, editor, data_owner]; nested exception is com.fasterxml.jackson.databind.exc.InvalidFormatException: Cannot deserialize value of type `io.grnry.project.services.ProjectRole` from String \"editofr\": not one of the values accepted for Enum class: [viewer, editor, data_owner]\n at [Source: (PushbackInputStream); line: 3, column: 5] (through reference chain: io.grnry.project.cotrollers.AssignUserToProjectRequest[\"roles\"]->java.util.ArrayList[0])",
  "type": "bad_parameter_value",
  "details": "uri=/projects/demo/users/4b40aa4b-d28b-4e77-bd89-11ca7dd1e63f"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="When user is not found in keycloak" %}
```
{
  "timestamp": "2021-09-01T15:17:41.246+00:00",
  "message": "HTTP 404 Not Found",
  "type": "entity_not_found",
  "details": "uri=/projects/demo/users/4b40aa4b-d28b-4e77-bd89-11ca7dd1e634"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="https://api.grnry.io" path="/projects/:project-name/users/:user-id" method="put" summary="Update user roles in the Project " %}
{% swagger-description %}
Adds the user to the specified project with roles set in request body

\




\


Requires the role 

`data_owner`
{% endswagger-description %}

{% swagger-parameter in="path" name="project-name" type="string" %}
Project name.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="user-id" type="string" %}
UUID like keycloak user identifier.

\



{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="roles" type="array" %}
An array of roles to be set for the specified user-id within the specified project. 
{% endswagger-parameter %}

{% swagger-response status="200" description="" %}
```
<Response body is empty>
```
{% endswagger-response %}

{% swagger-response status="400" description="When the passed role is not in the list of available roles" %}
```
{
  "timestamp": "2021-09-01T14:46:42.116+00:00",
  "message": "Passed object is not readalbe JSON parse error: Cannot deserialize value of type `io.grnry.project.services.ProjectRole` from String \"editofr\": not one of the values accepted for Enum class: [viewer, editor, data_owner]; nested exception is com.fasterxml.jackson.databind.exc.InvalidFormatException: Cannot deserialize value of type `io.grnry.project.services.ProjectRole` from String \"editofr\": not one of the values accepted for Enum class: [viewer, editor, data_owner]\n at [Source: (PushbackInputStream); line: 3, column: 5] (through reference chain: io.grnry.project.cotrollers.AssignUserToProjectRequest[\"roles\"]->java.util.ArrayList[0])",
  "type": "bad_parameter_value",
  "details": "uri=/projects/demo/users/4b40aa4b-d28b-4e77-bd89-11ca7dd1e63f"
}
```
{% endswagger-response %}

{% swagger-response status="404" description="When user is not found in keycloak" %}
```
{
  "timestamp": "2021-09-01T15:17:41.246+00:00",
  "message": "HTTP 404 Not Found",
  "type": "entity_not_found",
  "details": "uri=/projects/demo/users/4b40aa4b-d28b-4e77-bd89-11ca7dd1e634"
}
```
{% endswagger-response %}
{% endswagger %}
