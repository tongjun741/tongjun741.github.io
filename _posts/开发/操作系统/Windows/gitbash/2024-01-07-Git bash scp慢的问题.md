---
tags:
    - 操作系统
    - Windows
    - gitbash
---

在Git bash中使用scp命令向虚拟机传文件，大概只有4.5MB/s。

一直以为是虚拟机的问题，改了很多设置都没有效果。

偶然发现cmd下的scp速度不慢，查了很多资料说是Git bash自己的问题。

我把Git bash中的scp.exe文件替换成Windows 10自带的scp.exe后就解决问题了。

具体目录：

C:\Windows\System32\OpenSSH\scp.exe

=>

C:\Program Files\Git\usr\bin\scp.exe