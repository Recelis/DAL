+++
title = 'Authentication'
date = 2024-04-15T21:52:47.204Z
draft = true
+++

# JWT
(docs)[https://jwt.io/introduction/]

JSON Web Token is a way for transmitting information between parties as a JSON object. They are **Digitally Signed** so that they can be verified and trusted.

## JWT Structure

### Header
This will tell you what type of token it is (JWT) and signing algorithm which can be HMAC SHA256 or RSA.

```JSON
{
  "alg": "HS256",
  "typ": "JWT"
}
```
This is then **Base64Url** encoded.

### Payload
The payload contain the claims which are statements about the entity (usually the user). Three types of claims: **registered**, **public**, and **private**.

### Registered claims
These are optional predefined claims such as **iss**, **exp**, **sub**, **aud**. The full list and descriptions can be (here)[https://datatracker.ietf.org/doc/html/rfc7519#section-4.1].

## Signature
