---
tags:
    - 操作系统
    - Windows
    - WSL
---

下面是操作方法：

1. 首先关闭docker
2. 关闭所有发行版：
   `wsl --shutdown`
3. 将docker-desktop-data导出到D:\SoftwareData\wsl\docker-desktop-data\docker-desktop-data.[tar](https://so.csdn.net/so/search?q=tar&spm=1001.2101.3001.7020)（注意，原有的docker images不会一起导出）
   `wsl --export docker-desktop-data D:\SoftwareData\wsl\docker-desktop-data\docker-desktop-data.tar`
4. 注销docker-desktop-data：
   `wsl --unregister docker-desktop-data`
5. 重新导入docker-desktop-data到要存放的文件夹：D:\SoftwareData\wsl\docker-desktop-data\：
   `wsl --import docker-desktop-data D:\SoftwareData\wsl\docker-desktop-data\ D:\SoftwareData\wsl\docker-desktop-data\docker-desktop-data.tar --version 2`

只需要迁移docker-desktop-data一个发行版就行，另外一个不用管，它占用空间很小。

完成以上操作后，原来的%LOCALAPPDATA%/Docker/wsl/data/ext4.vhdx就迁移到新目录了： 重启docker，这下不用担心C盘爆满了！

参考：
https://docs.microsoft.com/zh-cn/windows/wsl/
https://docs.docker.com/docker-for-windows/wsl/



https://blog.csdn.net/thinszx/article/details/113665379





### 子系统迁移

因为虚拟机（Ubuntu20.04）默认安装在C盘，大量占用系统盘的空间，所以迁移到其他盘中

1. 查看安装的虚拟机

   ```bash
   wsl -l -v
   ```

2. 关闭所有正在运行的虚拟机

   ```bash
   wsl --shutdown
   ```

3. 对需要迁移的分发或虚拟机导出

   虚拟机名称：wsl -l -v可以查看名字，我的是Ubuntu-20.04

   文件导出路径：我导出在D盘（D:\wsl-Ubuntu-20.04.tar）

   ```bash
   wsl --export 虚拟机名称 文件导出路径
   ```

4. 卸载虚拟机（删除C盘的虚拟机数据）

   ```bash
   wsl --unregister 虚拟机名称
   ```

5. 导入新的虚拟机

   目标路径：新的虚拟机文件路径（理解为软件的安装路径就对了，我安装在D:\wsl\Ubuntu2004）

   虚拟机文件路径：第3步导出的文件（D:\wsl-Ubuntu-20.04.tar）

   --version 2：指定使用WSL2，如果填1就是指定使用WSL

   ```bash
   wsl --import 虚拟机名称 目标路径 虚拟机文件路径 --version 2
   ```

https://www.cnblogs.com/konghuanxi/p/14731846.html