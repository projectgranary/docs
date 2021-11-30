---
description: Upgrade from Granary 1.1 "Cliff" to Granary 1.2 "Dolores"
---

# Migration guide

## Project-scoping Related Upgrade Steps

### 1) Keycloak Admin Secret

Granary 1.2 ships with Projects as multi-tenancy concept. To be able to create such a project, the new [Project Management API](../../developer-reference/api-reference/project-api.md) needs to have Keycloak administrator access to create Keycloak clients. You can either deploy a secret with credentials of a Keycloak administrator user manually and set [the name and keys in the Management API Chart](https://github.com/syncier/grnry-management-api/blob/main/helm/values.yaml#L122) or you can use the automated deployment of the secret in the Granary Base Deployment. For that, you need to set the Credentials in the [Granary Base Deployment Chart](https://github.com/syncier/grnry-base-deployment/blob/master/helm/values.yaml#L44).

If you do not want to use the Keycloak super-admin user "keycloak", you can create a new user in realm "master" and assign all roles in client `grnry-realm`:

![](<../../.gitbook/assets/image (77).png>)

### 2) Add `project-api` Keycloak client

To manage access to the Project Management API up and running, we need to create a new client in Keycloak called `project-api`. It needs to have two roles:

1. `project_creator`
2. `user_viewer`

Find all details on the [Granary Access Clients](../identity-and-access-management/granary-access-clients.md#project-api) page.

In case you have a new installation and use the [Granary Base Helm chart](../installation/with-helm/granary-base-deployment.md), the Keycloak client creation will be done for you.

### 3) The `global` Project &#x20;

When upgrading to Granary 1.2, a default project with name `global` will get set up and all present Granary objects (Harvesters, Belts and EventTypes, see subpage on [Use Case Migration](projects-use-case-migration-concept.md)) will be assigned to it. For that, one has to provide a Kubernetes secret containing a technical Keycloak user that has got the `project_creator` role in client `project-api` assigned (see above). When using the [Granary Base Helm chart](../installation/with-helm/granary-base-deployment.md), this user gets created for you, otherwise please adapt secret related settings in Project Management API.

### 4) Ingress' needs Regex for Projects

With the project name being now part of all API's URLs, we need to ensure that the ingress objects are configured correctly. As Granary supports Nginx and Ambassador as ingress, we provide an example for both.

#### Nginx

Helm Chart values.yaml (relevant lines only):

```yaml
## Ingress - optional
ingress:
  enabled: false
  contextPath: "/(projects/.+/|.{0})belts($|/.*)"
  hosts: []
  annotations:
    kubernetes.io/ingress.class: nginx
  #  kubernetes.io/tls-acme: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /$1belts$2
  ## TLS configuration
  tls: []
  #- hosts: []
  #  secretName: lets-encrypt-tls
```

See lines 4 and 9 for the changes.

#### Ambassador

**Breaking Change:** Please comment out all Ambassador related Service annotations in your API Helm Chart values.yaml (relevant lines only):

```yaml
## Service configuration
service:
  type: ClusterIP
  externalPort: 80
  internalPort: 8080
  annotations: {}
  #  getambassador.io/config: |
  #    ---
  #    apiVersion: ambassador/v0
  #    kind:  Mapping
  #    name:  belt-api-mapping
  #    prefix: /belts
  #    rewrite: "/belts"
  #    service: http://grnry-belt-api
```

Instead deploy Ambassador (>v2) and create these `Mapping` resources for each API:

```yaml
apiVersion: getambassador.io/v3alpha1
kind:  Mapping
metadata: 
  name:  belt-api-mapping
  namespace: demo
spec: 
  hostname: '*'
  prefix: /belts
  rewrite: "/belts"
  service: http://grnry-belt-api
  timeout_ms: 30000
---
apiVersion: getambassador.io/v3alpha1
kind:  Mapping 
metadata: 
  name:  belt-api-projects-belts-mapping
  namespace: demo
spec: 
  hostname: '*'
  prefix: "/projects/(.*)/belts(.*)"
  prefix_regex: true
  regex_rewrite:
    pattern: "/projects/(.*)/belts(.*)"
    substitution: "/projects/\\1/belts\\2"
  service: http://grnry-belt-api
  timeout_ms: 30000
```

### 5) API Upgrade

Once you reach this point, you can start to upgrade the Harvester & Belt API components to the version denoted in the [release notes](../granary-release-notes/#granary-1.2.0-dolores-2021-11-15).

While doing, please read through the [API Migration Concept](api-migration-concept.md) to understand what will happen on the first startup.

### 6) Use Case Upgrade

Once the APIs are running in their respective Granary 1.2 version, we can continue updating the [Use Cases to Granary 1.2's project scoping](projects-use-case-migration-concept.md).

## Segment Related Upgrade Steps

### Belt API Helm Chart breaking changes

The new Belt API comes with differences in the Helm Chart because there are now two belt types that need default configuration in the chart:

* python-callback
* dbt-segment

#### Granary 1.1

Belt API values.yaml (only lines with breaking change):

```yaml
app:
  profileUrl: "http://grnry-profilestore-api"
  harvesterEventTypeUrl: "http://grnry-harvester-api/event-types"
  kafkaService: "grnry-kafka:9092"
  kafkaOutputTopic: "profile-update"
  encryptionMode: "true"
  encryptionSecret: "grnry-base-encryption-token"
  pullSecret: "grnry-dockerconfig"
  beltImageName: "hub.syncier.cloud/grnry/belt-extractor"
  beltClusterProxy:
  beltPackageRepository:
  beltSpec: # k8s config to pass to belts deployed by the API

    ## Liveness, Readiness of belt
    livenessProbe:
      httpGet:
        path: /
        port: 8000
      periodSeconds: 60
      timeoutSeconds: 10
      failureThreshold: 10

    readinessProbe:
      httpGet:
        path: /
        port: 8000
      periodSeconds: 60
      timeoutSeconds: 10
      failureThreshold: 10
    tolerations: []
    affinity: {}
    nodeSelector: {}
    securityContext:
      fsGroup: 1000
      runAsUser: 1000
```

#### Granary 1.2

Belt API values.yaml (only lines with breaking change):

```yaml
beltSpec: # k8s config to pass to belts deployed by the API
  pythonCallback:
    profileUrl: "http://grnry-profilestore-api"
    kafkaService: "grnry-kafka:9092"
    kafkaOutputTopic: "profile-update"
    encryptionMode: "true"
    encryptionSecret: "grnry-base-encryption-token"
    beltImagePullSecret: "grnry-dockerconfig"
    beltImagePullPolicy: "IfNotPresent"
    beltImageName: "hub.syncier.cloud/grnry/belt-extractor"
    beltImageTag: "2.1.0"
    beltClusterProxy:
    beltPackageRepository:

    livenessProbe:
      httpGet:
        path: /
        port: 8000
      periodSeconds: 60
      timeoutSeconds: 10
      failureThreshold: 10

    readinessProbe:
      httpGet:
        path: /
        port: 8000
      periodSeconds: 60
      timeoutSeconds: 10
      failureThreshold: 10

    tolerations: []
    affinity: {}
    nodeSelector: {}
    securityContext:
      fsGroup: 1000
      runAsUser: 1000

  dbtSegment:
    beltImageName: "hub.syncier.cloud/grnry/dbt-belt-image"
    beltImageTag: "1.0.0"
    beltImagePullSecret: "grnry-dockerconfig"
    beltImagePullPolicy: "IfNotPresent"
    restartPolicy: "Never"
    activeDeadlineSeconds: 1800 # kill job after 30 minutes

```

{% hint style="info" %}
Parameter `beltSpec.pythonCallback.beltImageTag` was introduced in Granary 1.2. To keep backward compatability, the extra environment variable `BELT_EXTRACTOR_VERSION` in the `extra.extraEnv` parameter takes precedences over the new parameter.
{% endhint %}

### DBT Segments

As seen in the previous step, the Belt API is now capable to deploy `dbt-segments`. Technically, these are Kubernetes CronJobs executing a DBT Docker image. The old [segment table creation templating language](https://docs.grnry.io/developer-reference/dataflow/segment-store/segment-table-creation) has been deprecated in Granary 1.2. The new DBT Docker image allows to deploy an arbitrary SQL `SELECT` statement as segment. Please refer to the new technical reference for details how to express the former `pivot` and `generic` segments as SQL `SELECT` statements.

## Update Granary Components

Update all remaining Granary components to their Granary 1.2 version as denoted in the [release notes](../granary-release-notes/). Bold versions indicate an update.

**That's it.**
