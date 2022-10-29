---
tags:
    - 浏览器
---

Chrome developer tools 的network中不显示cookie

为了安全原因默认是不显示的

```javascript
Solution
Go to chrome://flags/#site-isolation-trial-opt-out
Disable "Site isolation trial opt-out"
"RELAUNCH NOW"
Enjoy proper header information in dev tools

```

https://blog.ermer.de/2018/06/11/chrome-67-provisional-headers-are-shown/

