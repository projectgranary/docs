# API Reference

## **POSTMAN Collection**

In order to allow for an easier access to and a smoke test for Granary, a [POSTMAN collection](https://learning.getpostman.com/docs/postman/collections/intro_to_collections/) is provided for all Granary API endpoints. It includes sample requests for:

* [Spring Cloud Data Flow API](../../learning-grnry-1/data-in/how-to-run-a-harvester/getting-started.md)
* [Snowplow API](snowplow-api-endpoints.md)
* [Belt API](belt-api.md)
* [Event Store API](event-store-api.md)
* [Profile Store API](profile-store-api.md)
* [Harvester API](harvester-api.md)

To use it, the following two files need to be imported into POSTMAN:

{% file src="../../.gitbook/assets/grnry\_0.5.postman\_collection.json" caption="Granary Postman Collection" %}

{% file src="../../.gitbook/assets/grnry\_0.5.postman\_environment.json" caption="Granary Postman Collection Environment Values" %}

## **Authentication**

All REST API endpoints of GRNRY are protected via **OpenID Connect** \(OIDC\) which is based on **OAuth 2.0**. Therefore you need a **JSON Web Token** \(JWT [RFC 7519](https://tools.ietf.org/html/rfc7519)\) to access the API. You can find your Authentication Server at the `/auth` path under your GRNRY domain. For example `api.grnry.io/auth`

The Authorization header field needs to be send with every request. Since the JWT is used as a bearer token \([RFC 6750](https://tools.ietf.org/html/rfc6750)\) the header field looks like follows:

```text
Authorization: Bearer <token>
```

For more information on Granary's API authentication read the [identity and access management guide](../../operator-reference/identity-and-access-management/).

