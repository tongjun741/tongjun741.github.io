---
tags:
    - 前端
---

Disable gzip compression in chrome

38

Chrome doesn't seem to expose this setting, but what you can do is the following:

- get the ModHeader plugin for Chrome: https://chrome.google.com/webstore/detail/modheader/idgpnmonknjnojddfkpgkljpfnnfcklj?hl=en(or any other that allows you to fiddle with your browser request headers)

- set a new custom header accept-encoding to either:

- an empty value or

- gzip;q=0,deflate;q=0 either should work





https://stackoverflow.com/questions/18398022/disable-gzip-compression-in-chrome

