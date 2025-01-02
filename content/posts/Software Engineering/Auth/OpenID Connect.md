+++
title = 'OpenID Connect'
date = 2025-01-02T00:21:08.866Z
draft = true
+++

# OpenID Connect
[docs](https://openid.net/developers/how-connect-works/)

OIDC is a protocol for allowing users to authenticate themselves. The end user will sign in via the browser which allows the Client or `Relaying Party` (RP) sends a request to the OpenID Provider (OP) or `Identity Provider` (IDP). The OP authenticates the User and authorization and responds with an `Identity Token` and usually an `Access Token`.  The RP can send a request with the access token to the User device. Lastly the UserInfo endpoint will return `Claims` about the End-User.

This adds a layer over **OAUTH2**.
