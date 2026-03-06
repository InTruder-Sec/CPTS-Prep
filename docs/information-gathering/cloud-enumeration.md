---
title: Cloud Enumeration
excerpt: Cloud infrastructure discovery and enumeration techniques
hidden: false
---

# Cloud Enumeration

## Identifying Company Hosted Services

```bash
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done

blog.inlanefreight.com 10.129.24.93
inlanefreight.com 10.129.27.33
matomo.inlanefreight.com 10.129.127.22
www.inlanefreight.com 10.129.127.33
s3-website-us-west-2.amazonaws.com 10.129.95.250
```

## Google Dorks

### AWS S3 Buckets

**AWS:** `inurl:amazonaws.com intext:readmeio`

### Azure Blob Storage

**Azure:** `inurl:blob.core.windows.net intext:example.com`

## Cloud Enumeration Tools

- [domain.glass](https://domain.glass/) - Domain and cloud asset discovery
- [GrayhatWarfare S3 Buckets](https://buckets.grayhatwarfare.com/) - Public S3 bucket search engine
