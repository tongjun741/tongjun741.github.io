---
tags:
    - Docker
---

## 现象

`Docker`使用的是精简版，去除了`ps`、`top`、`pidof`等查看运行中的进程的命令。

## 解决方法

查看`/proc`目录，该目录记录着运行中的进程的`id`，以`id`为文件夹的名字，文件夹中的`exe`是一个超链接，指向的是运行该进程的命令。

```shell
ls -l /proc/*/exe
```

输出

```shell
lrwxrwxrwx 1 root root 0 Nov 19 18:46 /proc/1/exe -> /usr/java/openjdk-11/bin/java
lrwxrwxrwx 1 root root 0 Nov 21 13:37 /proc/49/exe -> /usr/bin/bash
lrwxrwxrwx 1 root root 0 Nov 21 13:37 /proc/self/exe -> /usr/bin/coreutils
lrwxrwxrwx 1 root root 0 Nov 21 13:37 /proc/thread-self/exe -> /usr/bin/coreutils
```

## 查看状态

```shell
cat /proc/1/status
```

https://www.zhangbj.com/p/1324.html