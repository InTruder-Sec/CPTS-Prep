---
title: SSL Enumeration
slug: ssl-enumeration
excerpt: SSL/TLS certificate enumeration for subdomain discovery
hidden: false
---

# SSL Enumeration

## Tools

- [crt.sh](https://crt.sh) - Certificate transparency logs search

## Certificate Transparency Search

### JSON Output

```bash
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq .
```

### Extract Unique Subdomains

If needed, we can also have them filtered by the unique subdomains:

```bash
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```

## DNS Resolution

### Getting Hosts (Avoiding 3rd Party Services)

```bash
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done
```

### Getting IP Addresses of Subdomains

```bash
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt;done
```

## DNS Records Enumeration

DNS records where we might find more hosts:

```bash
dig any inlanefreight.com
```
