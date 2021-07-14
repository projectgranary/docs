# Hints on naming of Segments

### Naming convention recommendations

It has proven that naming Segments with this pattern yields a good readability and overview:

`segment name = sgmt--<appID>--<usecaseID>--<content descriptor>`, where

1. `appID` is the name of the application owning the segment
2. `usecaseID` is the name of the concrete data product within the application that is being served by this segment
3. `content descriptor` is the short description of the segment's content, e.g. bounced customers, invalid requests, ...

If you denote the segment name in the environment variable `TARGET_SEGMENT_NAME`, please subsitute the `--` to become `_`.

### Technical limitations

Generally speaking, the Segment Managment API features an `id` and a `displayName`. The `id` is used as the technical identifier and is derived from the `displayName` parameter provided in the JSON payload when creating a [Segment](../../../developer-reference/api-reference/segment-management-api.md#create-segment-job) using a POST request. The only exception to this behaviour is if you provide an `id` in the POST request when creating a Harvester or an Event Type, e.g. `POST /segments/{id}`

The `displayName` property does not have technical restrictions. 

The following limitations apply to the `id` property of a segment:

* `id` may carry four to 134 characters
* this function is invoked if `id` is derived from `displayName`: `displayName.toLowerCase().replaceAll("[^a-zA-Z0-9\\-\\s]", "").trim().replaceAll("[\s]+"), ”-”) + "-" + 5 random characters`
* if created via API call `POST /segments/{id}` the ID must adhere to the DNS-1123 subdomain standard

