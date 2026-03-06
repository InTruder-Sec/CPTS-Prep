# Cloud enumeration

Company hosted services

```bash
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done

blog.inlanefreight.com 10.129.24.93
inlanefreight.com 10.129.27.33
matomo.inlanefreight.com 10.129.127.22
www.inlanefreight.com 10.129.127.33
s3-website-us-west-2.amazonaws.com 10.129.95.250
```

## Google Dorks

### AWS: **`inurl:amazonaws.com intext:readmeio`**

### Azure: `inurl:blob.core.windows.net intext:example.com`

[https://domain.glass/](https://domain.glass/)

[https://buckets.grayhatwarfare.com/](https://buckets.grayhatwarfare.com/)