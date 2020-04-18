---
title: "Identity Server Specifications & Flows"
linkTitle: "Identity Server Specifications & Flows"
date: 2019-08-26
weight: 1
---

## Identity Server Specifications & Flows
![](https://i.imgur.com/SECFI9O.png)

Identity Server is an OpenID Connect and OAuth 2.0 framework

It enables the following features in your applications:

**1. Authentication as a Service**
Centralized login logic and workflow for all of your applications (web, native, mobile, services). IdentityServer is an officially certified implementation of OpenID Connect.

**2. Single Sign-on / Sign-out**
Single sign-on (and out) over multiple application types.

**3. Access Control for APIs**
Issue access tokens for APIs for various types of clients, e.g. server to server, web applications, SPAs and native/mobile apps.

## Terminology

* **IdentityServer**
IdentityServer is an OpenID Connect provider - it implements the OpenID Connect and OAuth 2.0 protocols.

* **Resource Owner: User**
A user is a human that is using a registered client to access resources.

* **Client: Application**
A client is a piece of software that requests tokens from IdentityServer - either for authenticating a user (requesting an identity token) or for accessing a resource (requesting an access token). A client must be first registered with IdentityServer before it can request tokens.

* **Resource: API**
Resources are something you want to protect with IdentityServer - either identity data of your users, or APIs.
Every resource has a unique name - and clients use this name to specify to which resources they want to get access to.
Identity data Identity information (aka claims) about a user, e.g. name or email address.
APIs APIs resources represent functionality a client wants to invoke - typically modelled as Web APIs, but not necessarily.

* **Identity Token**
An identity token represents the outcome of an authentication process. It contains at a bare minimum an identifier for the user (called the sub aka subject claim) and information about how and when the user authenticated. It can contain additional identity data.

* **Access Token**
An access token allows access to an API resource. Clients request access tokens and forward them to the API. Access tokens contain information about the client and the user (if present). APIs use that information to authorize access to their data.
