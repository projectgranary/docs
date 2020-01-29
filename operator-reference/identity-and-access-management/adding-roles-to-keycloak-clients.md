---
description: >-
  This page provides a short guide on how to add your own client roles in
  keycloak and make them available to certain users or user groups.
---

# Adding Roles to Keycloak Clients



### Adding a New Role to Profile-Api-Client

* Go to "Clients" in your Keycloak side bar under "Configure".

![](../../.gitbook/assets/image%20%2814%29.png)

* Choose the client that needs the new role and click "Edit" under "Actions".

![](../../.gitbook/assets/image%20%2810%29.png)

* Select "Roles" tab and choose "Add Role".

![](../../.gitbook/assets/image%20%287%29.png)

* Enter the role name, e.g., `my_new_role`, and save it.

![](../../.gitbook/assets/image%20%2819%29.png)

### Assigning a Role to a User

* Go to "Users" in your  Keycloak side bar under "Manage".

![](../../.gitbook/assets/image%20%2820%29.png)

* Select "View all users".

![](../../.gitbook/assets/image%20%2823%29.png)

* choose the user you want to assign the role to and click "Edit" under "Actions".

![](../../.gitbook/assets/image%20%2815%29.png)

* Select "Role Mappings" Tab and click on the empty box under "Client Roles" to view all available clients and choose `profile-api`.

![](../../.gitbook/assets/image%20%2825%29.png)

* Select `my_new_role` and click "Add selected".

![](../../.gitbook/assets/image%20%285%29.png)

### Creating a User Group

* Go to "Groups" in your side bar.

![](../../.gitbook/assets/image%20%2821%29.png)

* Select "New".

![](../../.gitbook/assets/image%20%2818%29.png)

* Enter a name for your group and save.

![](../../.gitbook/assets/image%20%283%29.png)

### Adding a User to a Group

* Go to "Users" and choose "View all users" as described [above](). 
* Search for the user you want to add and click "Edit".
* Go to the "Groups" tab.
* Select the group you want to add the user to from "Available Groups" and click "Join".

![](../../.gitbook/assets/image%20%2826%29.png)

### Assigning a Role to a User Group

* Go to "Groups" in your side bar.

![](../../.gitbook/assets/image%20%2821%29.png)

* Choose an existing user group and click "Edit".
* Go to "Role Mappings" tab.
* Add the new role for the whole group as described [above ]()for a single user.

