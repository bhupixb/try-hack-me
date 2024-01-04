# TryHackMe DNS in Detail Lab Notes

Lab: [TryHackMe DNS in Detail](https://tryhackme.com/room/dnsindetail)

## Domain Hierarchy

### TLD (Top Level Domain)
The rightmost part of a domain name. For example, in `tryhackme.com`, `.com` is the TLD.

### Second Level Domain
In `tryhackme.com`, `tryhackme` is the second level domain. It is limited to 63 characters max, with allowed characters being a-z, 0-9, and hyphens. Domains cannot have two consecutive hyphens and should not start or end with a hyphen.

### Subdomain
The whole left part of a domain before the second level domain, separated by a dot. E.g., in `test.bhupixb.tryhackme.com`, `test.bhupixb` is the subdomain. The maximum length can be 63 characters, and the total length of a subdomain cannot exceed 253 characters.

## DNS Record Types

### A Records
These records resolve to IPv4 addresses. Example: `104.26.10.229`.

### AAAA Records
These records resolve to IPv6 addresses. Example: `2606:4700:20::681a:be5`.

### CNAME Records
These records resolve to another domain. For example, `store.tryhackme.com` resolves to `shop.shopify.com`. Sample output:
```bash
$ dig store.tryhackme.com

; <<>> DiG 9.10.6 <<>> store.tryhackme.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 44965
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;store.tryhackme.com.		IN	A

;; ANSWER SECTION:
store.tryhackme.com.	300	IN	CNAME	shops.myshopify.com.
shops.myshopify.com.	23	IN	A	23.227.38.74

;; Query time: 39 msec
;; SERVER: 172.16.64.51#53(172.16.64.51)
;; WHEN: Fri Jan 05 00:49:12 IST 2024
;; MSG SIZE  rcvd: 94
```

Read more about MX (for email server IP) & TXT (for storing some text, interesting project based on it: [DNS.toys](https://github.com/knadh/dns.toys)) records.

## What Happens When You Make a DNS Request?

1. Client (laptop) sends a request to the Recursive DNS Server (generally provided by ISP), which forwards it to the Root Name servers. The Root Name servers redirect the request to the appropriate TLD server.
2. The TLD server sends the request to the Authoritative DNS server, which then answers the query.
