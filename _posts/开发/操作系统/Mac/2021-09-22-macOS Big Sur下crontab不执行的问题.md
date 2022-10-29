---
tags:
    - 操作系统
    - Mac
---

### 问题起因

crontab -e
添加完定时任务以后，不执行

### 解决方法

给/usr/sbin/cron文件赋予完全磁盘权限

![image.png](/img-post/开发/操作系统/Mac/macOS Big Sur下crontab不执行的问题.assets/bVcRiub.png)



https://segmentfault.com/a/1190000039832776