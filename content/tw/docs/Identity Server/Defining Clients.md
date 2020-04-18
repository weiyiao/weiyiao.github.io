---
title: "Defining Clients"
linkTitle: "Defining Clients"
weight: 4
description: >
  Clients represent applications that can request tokens from your identity server.
---

The details vary, but you typically define the following common settings for a client:

* a unique client ID
* a secret if needed
* the allowed interactions with the token service (called a grant type)
* a network location where identity and/or access token gets sent to (called a redirect URI)
* a list of scopes (aka resources) the client is allowed to access 

## Client

### Default Scopes
> The OpenID Connect specification specifies a couple of [standard identity resources](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims").

| Name | Required/Optional | Description |
| -------- | -------- | -------- |
| openid | Required | Informs the Authorization Server that the Client is making an OpenID Connect request. If the *openid* scope value is not present, the behavior is entirely unspecified. |
| profile | Optional | This scope value requests access to the End-User's default profile Claims, which are: *name, familyname, givenname, middlename, nickname, preferredusername, profile, picture, website, gender, birthdate, zoneinfo, locale, and updatedat*. |
| email | Optional | This scope value requests access to the *email* and *emailverified* Claims. |
| address | Optional | This scope value requests access to the *address* Claim. |
| phone | Optional | This scope value requests access to the *phonenumber* and *phonenumberverified* Claims. |
| offline_access | Optional | This scope value MUST NOT be used with the OpenID Connect Implicit Client Implementer's Guide 1.0. </br>[See the OpenID Connect Basic Client Implementer's Guide 1.0 ](http://openid.net/specs/openid-connect-implicit-1_0.html#OpenID.Basic) for its usage in that subset of OpenID Connect. |

### GrantType

> Grant types are a way to specify how a client wants to interact with IdentityServer. The OpenID Connect and OAuth 2 specs define the following grant types:

| Name | Value |
| -------- | -------- |
| Implicit | implicit |
| ImplicitAndClientCredentials| implicit client_credentials |
| Code | authorization_code |
| CodeAndClientCredentials | authorization_code client_credentials |
| Hybrid | hybrid |
| HybridAndClientCredentials | hybrid client_credentials |
| DeviceFlow | urn:ietf:params:oauth:grant-type:device_code |
