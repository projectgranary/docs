---
description: A Migration Guide to update your Granary instance to be Keycloak 11 compatible
---

# Keycloak 11 Migration

Granary 1.0 ships with the newer Keycloak version 11. To ensure compatibility, two new client scopes "web-origins" and "roles" are needed in Keycloak's realm.json: 

```text
...
    "clientScopes":
    {
        "name": "web-origins",
        "description": "OpenID Connect scope for add allowed web origins to the access token",
        "protocol": "openid-connect",
        "attributes":
            {
                "include.in.token.scope": "true",
                "display.on.consent.screen": "false",
                "consent.screen.text": "",
            },
        "protocolMappers":
            [
                {
                    "name": "allowed web origins",
                    "protocol": "openid-connect",
                    "protocolMapper": "oidc-allowed-origins-mapper",
                    "consentRequired": false,
                    "config": {},
                },
            ],
    },
    {
        "name": "roles",
        "description": "OpenID Connect scope for add user roles to the access token",
        "protocol": "openid-connect",
        "attributes":
            {
                "include.in.token.scope": "true",
                "display.on.consent.screen": "true",
                "consent.screen.text": "${rolesScopeConsentText}",
            },
        "protocolMappers":
            [
                {
                    "name": "client roles",
                    "protocol": "openid-connect",
                    "protocolMapper": "oidc-usermodel-client-role-mapper",
                    "consentRequired": false,
                    "config":
                        {
                            "multivalued": "true",
                            "access.token.claim": "true",
                            "claim.name": "resource_access.${client_id}.roles",
                            "jsonType.label": "String",
                        },
                },
                {
                    "name": "audience resolve",
                    "protocol": "openid-connect",
                    "protocolMapper": "oidc-audience-resolve-mapper",
                    "consentRequired": false,
                    "config": {},
                },
            ],
    },
...
    "defaultDefaultClientScopes":
        ["role_list", "email", "profile", "web-origins", "roles"],
...
```

{% hint style="info" %}
For further information please have a look on Keycloak's in-house provided migration guides from [https://www.keycloak.org/docs/latest/upgrading/\#migrating-to-4-6-0](https://www.keycloak.org/docs/latest/upgrading/#migrating-to-4-6-0) to [https://www.keycloak.org/docs/latest/upgrading/\#migrating-to-11-0-0](https://www.keycloak.org/docs/latest/upgrading/#migrating-to-11-0-0)
{% endhint %}

