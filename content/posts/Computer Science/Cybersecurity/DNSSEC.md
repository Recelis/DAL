---
title: "DNSSEC"
date: 2024-04-14T11:16:45+10:00
draft: true
---
[docs](https://www.icann.org/resources/pages/dnssec-what-is-it-why-important-2019-03-05-en)

# DNS zone
The DNS data for a domain is called a zone.

# DNSSEC
DNS Security Extensions is a way of securing nameservers so that their data for each `DNS zone` is protected by `public key cryptography` and signed by the owner of the data. A `recursive resolver` looking up data in the zone will also get the `public key` which validates the authenticity of the DNS data.

The `chain of trust` is done when the public key of a zone's DNS isn't signed by the owner of the data but by the parent zone's private key. So a parent will be responsive for not only the list of a child zone's `authoritative name servers` but also the child zone's public key. If a user has DNSSEC turned on then if the signed data has been tampered with.
