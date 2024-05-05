---
title: "DNS"
date: 2024-04-03T21:17:55.166Z
---

(docs)[https://nullrouted.space/2021/10/27/understanding-dns-how-a-domain-name-gets-resolved-in-your-home-network/]
(docs)[https://nullrouted.space/2021/10/27/understanding-dns-how-a-domain-name-gets-resolved-in-your-home-network/]
(docs)[https://www.cloudflare.com/learning/dns/what-is-dns/]

# DNS (Domain Name System)

This is a system that allows any computer to get an Internet Protocol (IP) address of the website you want when given a `domain name`. It does this using the `TCP/IP model`.

There are a few steps when going from your computer to your ISP's `resolver` which is a `DNS recursive resolver` and basically the wide open internet (a phrase made up by me, not a technical phrase but it could already exist).

- Browser
  - asks the OS.
- The OS
  - asks its `stub resolver`
- stub resolver
  - asks for the `DNS resolver` from `DHCP (Dynamic Host Configuration Protocol)`.
- DNS resolver
  - sends a query to ISP's resolver.

## Recursive Resolvers

<quote>To simply put it: A recursive resolver is a DNS resolver that resolves a DNS query by going through hierarchical sets of authoritative nameservers until it gets an answer to the query.</quote> - NullRouted Space blog

You can use the `dig - DNS Information Groper` terminal utility.

```bash
    dig +trace +nodnssec domainname
    @149.112.121.10
```

The `@149.112.121.10` is the CIRA public resolver service. If this isn't passed in, then you'll use the /etc/resolv.conf DNS resolver value.

## Name servers

- Root nameserver
- TLD nameserver
  - finds the servers by TLD (Top Level Domain e.g. .com)
- Authoritative nameserver

## DNS Records

(docs)[https://www.cloudflare.com/en-gb/learning/dns/dns-records/]

### A Record

These records point to the IP address. They are considered the final record to get to your website.

| example.com record | type: |    value: |   TTL |
| ------------------ | :---: | --------: | ----: |
| @                  |   A   | 192.0.2.1 | 14400 |

### CNAME Records

These records point to a hostname. Typically they would work like this:
www.dogs.com -> dogs.com

| blog.example.com record | type: |                         value: |   TTL |
| ----------------------- | :---: | -----------------------------: | ----: |
| @                       | CNAME | is an alias of www.example.com | 32600 |
