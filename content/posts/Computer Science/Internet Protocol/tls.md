---
title: "Tls"
date: 2024-04-13T11:32:23+10:00
draft: true
---

https://www.khanacademy.org/computing/computers-and-internet/xcae6f4a7ff015e7d:online-data-security/xcae6f4a7ff015e7d:secure-internet-protocols/a/transport-layer-security-protocol-tls

Transport Layer Security (TLS) is something that sits on top of the TCP/IP transport protocols. It uses **symmetric encryption** and **public key encryption** for securely sending private data as well as authentication and message tampering detection.

It adds latency in Internet communications but the security benefits outweigh the cons.

It also replaces SSL so SSL and TLS are used interchangeably.

**Digital Certificates**

Before a sending computer sends a public key to a receiving computer when encrypting data, TLS requires the sender to verify the identity using a digital certificate.

Without this, attackers can intercept a request from one computer to another through **rogue access points** to do **man-in-the-middle attacks** (MITM).

The attacker can then send their own public key instead of the server’s public key. So then the attacker can override the value that the client sends to the server.

![MITM](/static/images/mitm%20attack.png)

**Certificate Authority**

So servers will send a public key and verify a digital certificate behind the public key.

To ensure that the public key is valid, a server will have to sign up with certificate authority who verifies the ownership of the domain, signs the certificate with their own name and public key, and provides a signed certificate back to the server.

There are a list of trusted certificate authorities that Apple trusts on iOS.

[List of available trusted root certificates in iOS 10 - Apple Support](https://support.apple.com/en-us/103631)

The chain of trust from user to server is thus:

![Trust Chain](/static/images/trust%20chain.png)

**intermediate certificate authorities**

There are multiple levels of certificate authorities. Root certificate authorities does not issue any digital certificates to servers. They instead issue them to **intermediate certificate authorities** who then pass to either another intermediate certificate authority or to the server directly.

![Trust Certs](/static/images/trusts%20certificate.png)

This increases the security of a system, because a root CA can invalidate the certificates of an intermediate CA if that intermediate CA has been compromised.

**TLS Process**

https://www.khanacademy.org/computing/computers-and-internet/xcae6f4a7ff015e7d:online-data-security/xcae6f4a7ff015e7d:secure-internet-protocols/a/transport-layer-security-protocol-tls

The TLS protocol sits on top of the TCP Protocol.

The client firstly notifies the server that it wants to use HTTPS and what type of encryption method.

Then the server will respond with a **digital certificate** as well as a public key.

The client will verify that this digital certificate is valid by matching it against a list of stored addresses. With this public key, it will use this to generate a private shared key (it doesn’t use **public key encryption** for the entire message because it would take too much time). So afterwards, it would then use **symmetric encryption** to encrypt the rest of the message and send off as a second encrypted “Finished” message.

![Client Key Exchange](/static/images/client%20key%20exchange.png)

The server will try to decrypt the “Finished” message with the shared key. If this is successful, then it will respond with its own “Finished” message. After this, the client will send off the entire encrypted message.

If a client needs to send off multiple HTTPS messages, it will do an abbreviated version of this process for future requests.

**TLS in the ISO/OSI model**

(docs)[https://medium.com/@Aircon/protocols-and-servers-2-tryhackme-thm-1cc0f927a9f7]

TLS will usually sit in the Presentation layer, allowing it to encrypt common application layer protocols.

For instance HTTP encrypted by TLS will become HTTPS.

![ISO-OSI](/static/images/iso-osi.png)
