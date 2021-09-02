---
description: >-
  This page contains a guide of how to stage a Granary Pipeline from a
  development stage to a higher stage, e.g. a test or production environment.
---

# Stage a Granary Pipeline

## 1. Storing Resources

### GrApp Directory Structure

For importing resources to a Granary environment using the Granary App \(GrApp\) importer job, the resources need to be stored as `.json` definition files in a directory structure like this:

```bash
grapp
├── belts
│   └── {belt-id}
│       └── belt.json
├── event-types
│   └── {event-type-name}
│       ├── event-type-version-1.json
│       ├── event-type-version-2.json
│       ├── event-type-version-3.json
│       ├── ...
│       └── eventstores
│           └── {event-store-name}
│               └── persister.json
├── harvesters
│   └── instances
│       └── {harvester-name}
│           └── harvester.json
└── segments
    └── {segment-id}
        └── segment.json
```

{% hint style="info" %}
Find the complete [staging specification](https://github.com/syncier/grnry-samples/tree/master/pipeline-staging-specs) in the Granary samples repository.
{% endhint %}

### Naming

In the above tree structure all nodes have a strict naming convention. Any node names not in curly braces are literals and must as such be equal in any GrApp structure. The names in the curly braces need to be replaced with the respective values \(e.g., the actual belt id of the defined belt\). The numbers referencing the event type versions must all be prefixed with "event-type-version-".

Below we show sample API requests using curl to export Granary's pipeline resources. Also, there is a [Postman collection](../developer-reference/api-reference/#postman-collection) available for download providing sample requests to all Granary APIs.

## 2. Exporting Resources

All Granary management APIs provide an "export" flag for GET requests on specific resources. If "export" is set to "true", the API will return a response body containing only those properties that were set by the user: API-generated properties and such properties that are set to default values will be omitted.

In each list view, Granary's UI provides a download button to obtain the needed `.json` definitions, too.

### 

### Event type

Since an [Event Type](data-in/how-to-run-a-harvester/event-types.md) might be available in multiple versions, you need to export all versions of the Event Type up to the version used in your Harvester. If version is `latest`, you need to include all of them. Get the definition by executing the following command for each version. Use Harvester API's [event-types endpoint](../developer-reference/api-reference/harvester-api/event-type-endpoints.md#get-a-specific-version-of-an-event-type).

#### Using the Command Line

```bash
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -o grapp/event-types/{event-type-name}/event-type-version-{version}.json \
 https://grnry-host/event-types/{event-type-name}/{version}?export=true
```

#### Using the UI

Choose the version in the drop down menu left to the "Export" button before you click on the latter. Repeat this for all versions.

![](../.gitbook/assets/image%20%2848%29.png)

### 

### Persister \(optional\)

The default persister will be created implicitly on Event Type creation. Yet, it is possible to export it, e.g., if there are any updates to the persister after creation in the source system. To get the definition, use the Harvester API's [persister endpoint](../developer-reference/api-reference/harvester-api/event-type-endpoints.md#get-persister-for-a-specific-event-type).

#### Using the Command Line

Currently, there is no UI export available for persister definitions.

```bash
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -o grapp/event-types/{event-type-name}/eventstores/{event-store-name}/persister.json \
 https://grnry-host/event-types/{event-type-name}/eventstores/{event-store-name}/persister?export=true
```

### 

### Harvesters

To export a [Harvester](data-in/how-to-run-a-harvester/harvesters.md) instance, use the Harvester API's [instance endpoints](../developer-reference/api-reference/harvester-api/harvester-instance-endpoints.md#get-harvester-details).

#### Using the Command Line

```bash
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -o grapp/harvesters/instances/{harvester-name}/harvester.json \
 https://grnry-host/harvesters/instances/{harvester-name}?export=true
```

#### Using the UI

![](../.gitbook/assets/image%20%2851%29.png)

### 

### Belts

To get a [Belt](using-data-in-granary/getting-started.md) definition, use the Belt API's [endpoints](../developer-reference/api-reference/belt-api.md#get-a-specific-belt-by-id).

#### Using the Command Line

```bash
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -o grapp/belts/{belt-id}/belt.json \
 https://grnry-host/belts/{belt-id}?export=true
```

#### Using the UI

![](../.gitbook/assets/image%20%2852%29.png)

### 

### Segment \(optional\)

To export a [Segment](../developer-reference/dataflow/segment-store/segment-table-creation.md), use the Segment Management API's [endpoints]().

#### Using the Command Line

Currently, there is no UI export available for segment job definitions.

```bash
curl -X GET -H "Content-Type: application/json" \
 -H "Authorization: Bearer <Access-Token>" \
 -o grapp/segments/{segment-id}/segment.json \
 https://grnry-host/segments/{segment-id}?export=true
```

## 3. Running the Import Job

After exporting all needed resources from your source Granary environment, you can use the [GrApp Importer Helm chart](../operator-reference/installation/with-helm/grapp-importer.md) to import the pipeline into your target Granary environment. Due to current restrictions of Helm 3, it is impossible to include a directory from outside the helm directory structure to a Helm deployment. That is why the whole "grapp" directory as described above needs to be located at the root of the Helm directory for now. There can only be one "grapp" directory in this structure. Hence, if you have multiple GrApp definitions, you would have to either merge them all together in one "grapp" directory or do the import process for each GrApp definition individually.

```bash
helm
├── Chart.yaml
├── grapp
│   └── ...
├── templates
│   ├── _helpers.tpl
│   ├── grapp-configmap.yaml
│   └── job.yaml
└── values.yaml
```

To download the GrApp Importer Helm chart, in Helm 3 use the `helm pull` command:

```bash
helm pull grnry-stable/grnry-grapp-importer --untar
```

Start the job via `helm install` command:

```bash
helm install grnry-importer-my-grapp \
path/to/grapp-importer/helm \
-f path/to/grapp-importer-values.yaml \
--set fullnameOverride=grnry-importer-my-grapp \
```

 

