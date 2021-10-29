# Hints on naming of Harvesters & Event Types

### Naming convention recommendations

#### Event Type

There are two "schools of event type naming":

1. Name the event type according to the owner of the data, i.e. `event type name = <appID>-<usecaseID>`, where
   1. `appID` is the name of the application owning the data within the pipelines of this specific event type
   2. `usecaseID` is the name of the concrete data product within the application that is being served by the event type
2. Name the event type according to the data it provides, i.e. `event type name = <name of data source>`, e.g. `Snowplow Tracking Data`, `OpenSky Data` or `ABS Master Data`

#### Harvesters

It has proven that naming Harvesters with this pattern yields a good readability and overview:

`harvester name = hvs--<source type name>--<event type name>`

Some examples:

* hvs--jdbc--abs-contracts
* hvs--sp--tracking-data (sp == snowplow)
* hvs--s3-document-data

### Technical limitations

Generally speaking, the Harvester API features a `name` and a `displayName`. The `name` is used as the technical identifier and is derived from the `displayName` parameter provided in the JSON payload when creating a [Harvester](../../../developer-reference/api-reference/harvester-api/harvester-instance-endpoints.md#create-harvester) / [Event Type](../../../developer-reference/api-reference/harvester-api/event-type-endpoints.md#create-an-event-type) using POST request. The only exception to this behaviour is if you provide a `name` in the POST request when creating a Harvester or an Event Type, e.g. `POST /harvesters/instances/{name}`

The `displayName` property has a maximum of 256 characters.&#x20;

#### Event Types

The following limitations apply to the `name` property of Event Types:

* `name` may carry max. 20 characters
* this function is invoked if `name` is derived from `displayName`:
  * `displayName.replaceAll("[^a-zA-Z0-9\\-\\s]", "").trim().replaceAll("[\s]+", "-")`
* if created via API call `POST /event-types/{name}`, this regular expression applies:\
  __`[a-zA-Z0-9\-]*$`
  * if provided `name` is >20 characters, the API call returns `400 Bad Request`
* if cleaned `name` already exist, four random charaters are appended
* `name` is stored case-insensitive

#### Harvesters

The following limitations apply to the `name` property of Harvesters:

* `name` may carry max. 20 characters
* this function is invoked if `name` is derived from `displayName`:
  * `displayName.replaceAll("[^a-zA-Z0-9\\-\\s]", "").trim().replaceAll("[\s]+", "-")`
* if created via API call `POST /harvesters/instances/{name}` this regular expression applies:\
  __`[a-zA-Z0-9\-]*$`
  * if provided `name` is >20 characters, the API call returns `400 Bad Request`
* if cleaned `name` already exist, four random charaters are appended
* `name` is stored case-insensitive
