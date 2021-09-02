# \[Projects\] API Migration Concept

The new Granary 1.2 release adds a separation layer to the Granary objects: **projects**

A project is a container for all kinds of Granary Objects \(Belts, Harvesters, Persisters, Event Types\) and users. After this upgrade, there will be an isolation between the different projects in a way that components can only make use of each other if they are in the same project. A user can participate in multiple projects, but the Granary Objects will be bound to one project only.

When migrating to the new project approach of Granary, it is highly recommended to update both APIs \(Harvester API and Belt API\) together.

{% hint style="danger" %}
The updated Belt API won't be able to create new Belts before the Harvester API is updated, whereas the updated Harvester API won't be able to delete Event Types before the Belt API is updated.
{% endhint %}

After the migration, all existing Granary objects will be located in the `global` project. This means, the project `global` must exist in Keycloak. To achieve this, run the following call against Projects API:

```bash
curl -X POST --location "http://{{host}}/projects" \ 
-H "Content-Type: application/json" \ 
-H "Authorization: {{token}}" \ 
-d "{ \"displayName\": \"global\", \"description\": \"default project\" }"
```

It also needs to be ensured that all Granary users are "viewer" members of the `global` project. In Keycloak, this can be easiest achieved by adding the "viewer" role of client `global` to the default user group.

## Belt API

All existing Belts will be moved to the `global` project automatically. The `/belts` URL with no 'projects' parameter will be routed to the `global` project.

### Ingress

Add the following properties to the 'ingress' section of your `values.yaml` file  to support both `/projects/{project-name}/belts` and `/belts` \(remark: the 'annotations' section differs per ingress controller\).

{% code title="values.yaml" %}
```text
...
ingress:
    enabled: true
    contextPath: "/(projects/.+/|.{0})belts($|/.*)"
    annotations:
        nginx.ingress.kubernetes.io/rewrite-target: /$1belts$2
...
```
{% endcode %}

{% hint style="warning" %}
The "editor" and "viewer" roles for Belt access have been deprecated and do not work any longer. The respective properties have been removed from the Belt model.
{% endhint %}

## Harvester API

All existing Harvesters, Event Types and Persisters will be moved to the `global` project automatically. The deprecated `/harvesters` URL with no 'projects' parameter will be routed to the `global` project.

The deprecated `/event-types` URL with no 'projects' parameter will be routed to the `global` project for modifying operations \(POST, PUT, DELETE\) and for Persister access \(GET\). For other GET requests, the unscoped URL will query all Event Types from all projects but return a limited view.

### Ingress

Add the following properties to the 'ingress' section of your `values.yaml` file to support both `/projects/{project-name}/(harvesters|event-types)` and `/(harvesters|event-types)`\(remark: the 'annotations' section differs per ingress controller\).

{% code title="values.yaml" %}
```text
ingress:
    enabled: true
    contextPaths:
        - "/(projects/.+/|.{0})(harvesters)($|/.*)"
        - "/(projects/.+/|.{0})(event-types)($|/.*)"
    annotations:
        nginx.ingress.kubernetes.io/rewrite-target: /$1$2$3
```
{% endcode %}

{% hint style="warning" %}
The "consumer" and "editor" roles for Event Type access have been deprecated. The respective properties have been removed from the Event Type model.
{% endhint %}

## Reaper

In Granary 1.2, the Reaper interacts with the Harvester API's event type endpoints via the `global` project only. This means, please ensure that the Reaper's technical user in Keycloak has got **"editor"** permissions in project `global`.

