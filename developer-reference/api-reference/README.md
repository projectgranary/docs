# API Reference

## **Authentication**

All REST API endpoints of GRNRY are protected via **OpenID Connect** \(OIDC\) which is based on **OAuth 2.0**. Therefore you need a **JSON Web Token** \(JWT [RFC 7519](https://tools.ietf.org/html/rfc7519)\) to access the API. You can find your Authentication Server at the `/auth` path under your GRNRY domain. For example `demo.grnry.io`**`/auth`**

The Authorization header field needs to be send with every request. Since the JWT is used as a bearer token \([RFC 6750](https://tools.ietf.org/html/rfc6750)\) the header field looks like follows:

```text
Authorization: Bearer <token>
```

For more information on Granary's API authentication read the [identity and access management guide](../../operator-reference/identity-and-access-management.md).

