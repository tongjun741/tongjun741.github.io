---
tags:
    - Docker
---

docker images 导入导出

导出镜像

如果要存出镜像到本地文件，可以使用docker save命令。例如，存出本地的ubuntu:14.04镜像为文件ubuntu_14.04.tar：

$ sudo docker save -o /home/user/images/ubuntu_14.04.tar ubuntu:14.04



导入镜像

可以使用docker load从存出的本地文件中再导入到本地镜像库，例如从文件ubuntu_14.04.tar导入镜像到本地镜像列表，如下所示：

$ sudo docker load --input ubuntu_14.04.tar

————————————————

版权声明：本文为CSDN博主「王彦清」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/u011365831/java/article/details/81430513

