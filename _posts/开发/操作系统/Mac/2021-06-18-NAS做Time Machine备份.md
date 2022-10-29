---
tags:
    - 操作系统
    - Mac
---

NAS做Time Machine备份

需要让服务器支持AFP协议就好了



```javascript
sudo apt-get install netatalk avahi-daemon
sudo vim /etc/netatalk/AppleVolumes.default

```

改成:

```javascript
:DEFAULT: options:upriv,usedots
:DEFAULT: options:upriv,usedots

```



重启一下服务，在Macbook上简单配置一下，就能在时间机器上看到对应的磁盘

![](../../../../_resources/3f70f1a1a52c442691a5d8a0df0501bf.png)



自己拥有一台服务器可以做哪些很酷的事情？ - 彭家进的回答 - 知乎
https://www.zhihu.com/question/40854395/answer/583279471

