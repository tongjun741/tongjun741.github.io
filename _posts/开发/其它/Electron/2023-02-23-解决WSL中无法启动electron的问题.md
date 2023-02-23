---
tags:
    - 其它
    - Electron
---

Electron 给出错误信息：
`/usr/local/lib/node_modules/electron/dist/electron: error while loading shared libraries: libatk-1.0.so.0: cannot open shared object file: No such file or directory.`



```
sudo apt install libnss3-dev libatk1.0-0 libatk-bridge2.0-0 libgdk-pixbuf2.0-0 libgtk-3-0 libgbm-dev  -y
```

https://github.com/electron/electron/issues/26673