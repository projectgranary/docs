# API Reference

## **POSTMAN Collection**

In order to allow for an easier access to Granary, a [POSTMAN collection](https://learning.getpostman.com/docs/postman/collections/intro_to_collections/) is provided for all Granary API endpoints. It includes sample requests for:

* [Snowplow API](snowplow-api-endpoints.md)
* [Project API](project-api.md)
* [Harvester API](harvester-api/)
* [Belt API](belt-api.md)
* [Event Store API](event-store-api.md)
* [Profile Store API](profile-store-api.md)

To use it, the following collection file needs to be imported into POSTMAN:

{% file src="../../.gitbook/assets/grnry\_1.0\_aretha.postman\_collection.json" caption="grnry\_1.0\_aretha.postman\_collection.json" %}

The collection automatically fetches an auth token \(see Authentication paragraph below\) and refreshes the expired token before each request. This is done with a [pre-request script](https://learning.postman.com/docs/writing-scripts/pre-request-scripts/) in the collection itself.

The existing [collection variables](https://learning.postman.com/docs/sending-requests/variables/#defining-collection-variables) are:

| Name | Default Value |
| :--- | :--- |
| client\_id | profile-api |
| realm | grnry |
| username | username |
| password | password |
| auth\_url | https://demo.grnry.io |
| snowplow\_url | https://demo.grnry.io |
| harvester\_url | https://demo.grnry.io |
| event\_store\_url | https://demo.grnry.io |
| belt\_url | https://demo.grnry.io |
| profile\_store\_url | https://demo.grnry.io |
| segment\_mgmt\_url | https://demo.grnry.io |

In order to overwrite them create a new Environment in Postman and update the necessary values. A common use case is to have separate Environments for development and production stages as the URLs and credentials differ between the stages. 

Follow this [link](https://learning.postman.com/docs/sending-requests/variables/#defining-global-and-environment-variables) for an explanation how to create a new Environment. More information about the scope of variables can be found [here](https://learning.postman.com/docs/sending-requests/variables/#variable-scopes).

## **Authentication**

All REST API endpoints of GRNRY are protected via **OpenID Connect** \(OIDC\) which is based on **OAuth 2.0**. Therefore you need a **JSON Web Token** \(JWT [RFC 7519](https://tools.ietf.org/html/rfc7519)\) to access the API. You can find your Authentication Server at the `/auth` path under your GRNRY domain. For example `api.grnry.io/auth`

The Authorization header field needs to be send with every request. Since the JWT is used as a bearer token \([RFC 6750](https://tools.ietf.org/html/rfc6750)\) the header field looks like follows:

```text
Authorization: Bearer <token>
```

For more information on Granary's API authentication read the [identity and access management guide](../../operator-reference/identity-and-access-management/).

