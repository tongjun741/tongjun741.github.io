---
tags:
    - 其它
    - PHP
---

报错：

```
configure: error: not found. Please provide a path to MagickWand-config or Wand-config program.
make: *** No targets specified and no makefile found. Stop.
error |-Successify --- 命令已执行! ---
```





ssh下执行以下命令后再次尝试安装：

```
apt-get update
apt install libmagickwand-dev libmagickcore-dev
```



https://www.bt.cn/bbs/thread-102600-1-1.html