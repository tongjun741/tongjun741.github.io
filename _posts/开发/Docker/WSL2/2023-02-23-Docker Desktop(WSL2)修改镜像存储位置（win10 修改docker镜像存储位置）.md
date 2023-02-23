---
tags:
    - Docker
    - WSL2
---

```
原文链接：https://blog.csdn.net/weixin_45859850/article/details/115387169

wsl --export docker-desktop D:\docker-desktop.tar
wsl --export docker-desktop-data D:\docker-desktop-data.tar

wsl --import docker-desktop D:\docker\docker-desktop D:\docker-desktop.tar --version 2
wsl --import docker-desktop-data D:\docker\docker-desktop-data D:\docker-desktop-data.tar --version 2
————————————————
版权声明：本文为CSDN博主「rang#」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_45859850/article/details/115387169
```



Docker Desktop(WSL2)修改镜像存储位置
因为我使用的是WSL2的版本，所以docker desktop在安装的时候创建两个wsl子系统，使用命令wsl -l -v --all:


docker-desktop是存放程序的，docker-desktop-data是存放镜像的，这两个wsl子系统都是默认放在系统盘的。

现将这2个存储文件迁移至其他盘(比如:D:\system\wsl)的流程如下:

1.导出wsl子系统镜像:

wsl --export docker-desktop D:\system\wsl\docker-desktop\docker-desktop.tar
wsl --export docker-desktop-data D:\system\wsl\docker-desktop-data\docker-desktop-data.tar
1
2
2.删除现有的wsl子系统：

wsl --unregister docker-desktop
wsl --unregister docker-desktop-data
1
2
3.重新创建wsl子系统：

wsl --import docker-desktop D:\system\wsl\docker-desktop D:\system\wsl\docker-desktop\docker-desktop.tar --version 2
wsl --import docker-desktop-data D:\system\wsl\docker-desktop-data D:\system\wsl\docker-desktop-data\docker-desktop-data.tar --version 2
1
2
文章知识点与官方
————————————————
版权声明：本文为CSDN博主「#_##」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_45859850/article/details/115387169