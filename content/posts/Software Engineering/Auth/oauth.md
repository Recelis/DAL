+++
title = 'OAuth'
date = 2024-11-18T21:17:17.787Z
draft = true
+++

# OAUTH
[docs](https://datatracker.ietf.org/doc/html/rfc6749)

This is a framework for authorization users to obtain limited access to an HTTP service.

Previously, any third-party application logging into a service would store the resource owner's credentials as a password in clear-text. Think of this as accessing a database with the admin account and password. This is obviously unsafe. Servers would support password authentication which is unsafe. Third-party applications have broad access to all the protected resources. No restriction of duration or subset of resources. Resource owners cannot revoke access to an individual third party without changing their own password and removing access to everyone else. Finally, if any third-party application was compromised, then everything would be compromised too.

OAuth fixes this by adding an `authorization layer` 

## OAUTH2
This is the latest version of the oauth framework. There are a number of different flows as defined by this framework.

## Different Flows in OAuth2

The different flows are:

- `Authorization Code flow`
- `Client Credentials Flow`
- `Implicit Flow`
- `Password Grant Flow`

Only the `Authorization Code Flow` includes a refresh token.

### Authorization Code Flow

Basically, clients will request access from a resource owner and be given an `access token` by an `authorization server`. The client will then use the access token to access protected resources on the `resource server`.

Clients will request access from the resource owner and be granted an `access token` which contains the scope, lifetime, and other access attributes by an `authorization server` with approval of the `resource owner`. Client will then use access token to access protected resources on the `resource server`.

  +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+

#### Roles
- `Resource owner`
- `Resource server`
- `Client`
- `Authorization server`



