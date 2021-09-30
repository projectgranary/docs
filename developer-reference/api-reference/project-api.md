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

{% api-method method="post" host="https://api.grnry.io" path="/projects" %}
{% api-method-summary %}
Create Project
{% endapi-method-summary %}

{% api-method-description %}
Creates a project. A technical name for the project entity will be derived from given `displayName` by removing all special characters, replacing white spaces with hyphens and limiting the length to 20 characters. Should the created project name be already in use, the last four characters will be replaced by a suffix of numbers.  
Granary projects are represented as Keycloak client with specific attributes. On _Create Project API call_  the following will be created:  
    • Keycloak client entry with attributes \(client\_type, description, display\_name\)  
    • Database schema      
    • Database user, created user will be set as owner of created schema  
    • Kubernetes secret where database user credentials are stored   
  
To be able to create a project, the `project_creator` role is required in client `project-api`. Otherwise, _forbidden_ error code will be returned
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="deploy" type="string" required=false %}
Coma separated list of deploy options.   
Supported options:   
- sqlpad: SqlPad instance will be deployed and connected to corresponding project schema  
  
Example:   
/projects?deploy=sqlpad  
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="description" type="object" required=false %}
Project description
{% endapi-method-parameter %}

{% api-method-parameter name="displayName" type="object" required=true %}
Human readable name. A technical name will be derived from it.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Cake successfully retrieved.
{% endapi-method-response-example-description %}

```
{
  "name": "demo_granary_project",
  "displayName": "Demo Granary Project",
  "description": "Demo Granary Project description"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Project display name is not valid. As an example when 'pg' chars are not allowed at the begging
{% endapi-method-response-example-description %}

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
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
When token doesn't have `project-api` client access with `project-creator` role.
{% endapi-method-response-example-description %}

```
{
  "timestamp": "2021-08-31T13:10:29.446+00:00",
  "message": "Access forbidden due to missing roles.",
  "type": "entity_not_accessible",
  "details": "uri=/projects"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/projects/:project-name" %}
{% api-method-summary %}
Get Project details
{% endapi-method-summary %}

{% api-method-description %}
Returns all details of a given project.   
  
Requires the role: any of `data_owner` , `viewer`,  `editor`
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project-name" type="string" required=true %}
Project name.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

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
  }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
When roles cannot be loaded for the specific project. Also applicable to the case when project is not found because of roles missing.
{% endapi-method-response-example-description %}

```
{
  "timestamp": "2021-09-01T15:01:32.873+00:00",
  "message": "Access forbidden due to missing roles.",
  "type": "entity_not_accessible",
  "details": "uri=/projects/demo"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="put" host="https://api.grnry.io" path="/projects/:project-name" %}
{% api-method-summary %}
Update Project
{% endapi-method-summary %}

{% api-method-description %}
Updates a project. All body parameters are optional. Empty fields will be set to "", missing fields will remain unchanged.   
  
Requires the role `data_owner`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project-name" type="string" required=true %}
Project name.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter type="string" name="displayName" %}
Project display name.
{% endapi-method-parameter %}

{% api-method-parameter type="string" name="description" %}
Project description.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "name": "demo_granary_project",
  "displayName": "Demo Granary Project",
  "description": "new description"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
When roles cannot be loaded for the specific project. Also applicable to the case when project is not found because of roles missing.
{% endapi-method-response-example-description %}

```
{
  "timestamp": "2021-08-31T13:10:29.446+00:00",
  "message": "Access forbidden due to missing roles.",
  "type": "entity_not_accessible",
  "details": "uri=/projects/{projectName}"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/projects/:project-name/users" %}
{% api-method-summary %}
Get Members of the Project
{% endapi-method-summary %}

{% api-method-description %}
Returns an array of users assigned to the specified project.   
User details contains: 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project-name" type="string" required=true %}
Project name.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

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
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "timestamp": "2021-08-31T13:10:29.446+00:00",
  "message": "Access forbidden due to missing roles.",
  "type": "entity_not_accessible",
  "details": "uri=/projects/{projectName}/users"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/projects" %}
{% api-method-summary %}
Get a User's Projects
{% endapi-method-summary %}

{% api-method-description %}
Returns an array of projects available for currently authenticated user.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter required=true name="Authentication" type="string" %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
[
  {
    "name": "demo_0000",
    "displayName": "Demo",
    "description": "Demo Granary Project description"
  },
  {
    "name": "data_project",
    "displayName": "Data Project",
    "description": "Data Project description"
  }
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.grnry.io" path="/users" %}
{% api-method-summary %}
Get Users to Add them to Project
{% endapi-method-summary %}

{% api-method-description %}
Returns an array of users available in Granary to be assigned.  
Requires role `user_viewer` in `project-api` client.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="search" type="string" required=true %}
Search string for users.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
for `search=jo`
{% endapi-method-response-example-description %}

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
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
When token doesn't have the role `user_viewer` in client `project-api`.
{% endapi-method-response-example-description %}

```
{
  "timestamp": "2021-08-31T13:10:29.446+00:00",
  "message": "Access forbidden due to missing roles.",
  "type": "entity_not_accessible",
  "details": "uri=/users"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.grnry.io" path="/projects/:project-name/users/:user-id" %}
{% api-method-summary %}
Add User to Project
{% endapi-method-summary %}

{% api-method-description %}
Adds the user to the specified project with roles set in request body.  
  
Requires the role `data_owner`
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="user-id" type="string" required=true %}
UUID like keycloak user identifier.
{% endapi-method-parameter %}

{% api-method-parameter name="project-name" type="string" required=true %}
Project name.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" required=true type="string" %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter type="array" name="roles" required=true %}
An array of roles to be assigned for specified user-id
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
<Response body is empty>
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
When the passed role is not in the list of available roles
{% endapi-method-response-example-description %}

    {
      "timestamp": "2021-09-01T14:46:42.116+00:00",
      "message": "Passed object is not readalbe JSON parse error: Cannot deserialize value of type `io.grnry.project.services.ProjectRole` from String \"editofr\": not one of the values accepted for Enum class: [viewer, editor, data_owner]; nested exception is com.fasterxml.jackson.databind.exc.InvalidFormatException: Cannot deserialize value of type `io.grnry.project.services.ProjectRole` from String \"editofr\": not one of the values accepted for Enum class: [viewer, editor, data_owner]\n at [Source: (PushbackInputStream); line: 3, column: 5] (through reference chain: io.grnry.project.cotrollers.AssignUserToProjectRequest[\"roles\"]->java.util.ArrayList[0])",
      "type": "bad_parameter_value",
      "details": "uri=/projects/demo/users/4b40aa4b-d28b-4e77-bd89-11ca7dd1e63f"
    }
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
When user is not found in keycloak
{% endapi-method-response-example-description %}

```
{
  "timestamp": "2021-09-01T15:17:41.246+00:00",
  "message": "HTTP 404 Not Found",
  "type": "entity_not_found",
  "details": "uri=/projects/demo/users/4b40aa4b-d28b-4e77-bd89-11ca7dd1e634"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="put" host="https://api.grnry.io" path="/projects/:project-name/users/:user-id" %}
{% api-method-summary %}
Update user roles in the Project 
{% endapi-method-summary %}

{% api-method-description %}
Adds the user to the specified project with roles set in request body  
  
Requires the role `data_owner`
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project-name" type="string" required=true %}
Project name.
{% endapi-method-parameter %}

{% api-method-parameter type="string" name="user-id" required=true %}
UUID like keycloak user identifier.  
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter type="string" name="Authentication" required=true %}
Authentication token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter type="array" required=true name="roles" %}
An array of roles to be set for the specified user-id within the specified project. 
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
<Response body is empty>
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
When the passed role is not in the list of available roles
{% endapi-method-response-example-description %}

    {
      "timestamp": "2021-09-01T14:46:42.116+00:00",
      "message": "Passed object is not readalbe JSON parse error: Cannot deserialize value of type `io.grnry.project.services.ProjectRole` from String \"editofr\": not one of the values accepted for Enum class: [viewer, editor, data_owner]; nested exception is com.fasterxml.jackson.databind.exc.InvalidFormatException: Cannot deserialize value of type `io.grnry.project.services.ProjectRole` from String \"editofr\": not one of the values accepted for Enum class: [viewer, editor, data_owner]\n at [Source: (PushbackInputStream); line: 3, column: 5] (through reference chain: io.grnry.project.cotrollers.AssignUserToProjectRequest[\"roles\"]->java.util.ArrayList[0])",
      "type": "bad_parameter_value",
      "details": "uri=/projects/demo/users/4b40aa4b-d28b-4e77-bd89-11ca7dd1e63f"
    }
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
When user is not found in keycloak
{% endapi-method-response-example-description %}

```
{
  "timestamp": "2021-09-01T15:17:41.246+00:00",
  "message": "HTTP 404 Not Found",
  "type": "entity_not_found",
  "details": "uri=/projects/demo/users/4b40aa4b-d28b-4e77-bd89-11ca7dd1e634"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

