---
tags:
    - NAS
    - 蜗牛星际-黑群晖
---



 https://www.xgboke.com/2121.html



### 文件信息

资源名称：WES7_X86_X64_CN_SP1.iso

应用平台：Windows

资源版本：Service Pack1

资源大小：1.6GB

下载次数：1016

https://pan.baidu.com/share/init?surl=Uket8Y4NMJ5lCl8xUFcy4g

密码：wult



# 利用群晖VMM虚拟机安装Windows

 2019年7月26日*10:28:40*[ 4](https://www.xgboke.com/2121.html#comments) 25.4K 593字阅读1分58秒

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602435914.png)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602435914.png)

上一篇文章介绍了通过群晖Virtual Machine Manager安装[旁路由](https://www.xgboke.com/2081.html)，这次将在Virtual Machine Manager中安装Windows系统。本文以[WES7](https://www.xgboke.com/1317.html)的ISO格式为例进行安装，我觉得在群晖VMM中安装ISO格式最为简单、直观，其他ESD、WIM格式的Windows系统还需要引导工具。只需几个步骤就可以在Virtual Machine Manager上运行Windows。提示：如果是黑群用户请提前开启主板Bios的VT选项。

创建系统镜像文件从PC上传或Diskstation选择

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072601554721.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072601554721.jpg)

选择存储空间

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072601561293.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072601561293.jpg)



大概几分钟时间就可以完成创建

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072601563127.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072601563127.jpg)

选择虚拟机-新增-Microsoft Windows

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072601582066.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072601582066.jpg)

配置虚拟机规格

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072601583389.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072601583389.jpg)

选择启动ISO文件和其它ISO文件（Guest Tool（驱动）并创建虚拟盘）

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072601593216.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072601593216.jpg)

配置网络

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072601594548.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072601594548.jpg)

其他设置

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072601595826.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072601595826.jpg)

指定权限

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602000985.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602000985.jpg)

预览设置完成创建

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602002077.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602002077.jpg)

点开机启动虚拟机

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602010719.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602010719.jpg)

开机后点击连接（通过浏览器预览安装）

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602015341.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602015341.jpg)



[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602021158.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602021158.jpg)

安装完成后，打开计算机-选择Guest Tool所在的驱动器并安装

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602024182.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602024182.jpg)[![利用群晖VMM虚拟机安装Windows](利用群晖VMM虚拟机安装Windows WES7.assets/2019072602025152.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602025152.jpg)[![利用群晖VMM虚拟机安装Windows](利用群晖VMM虚拟机安装Windows WES7.assets/2019072602025927.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602025927.jpg)

开启远程桌面连接

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602031756.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602031756.jpg)

关闭防火墙或把远程桌面加入“例外”

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602201627.png)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602201627.png)



手动更改IP地址

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602033141.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602033141.jpg)



用户账户设置密码（远程桌面需要用到）

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602034783.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602034783.jpg)



路由器设置端口映射（远程桌面默认端口3389）

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602035813.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602035813.jpg)



通过路由器绑定的域名或IP地址远程连接桌面

[![利用群晖VMM虚拟机安装Windows](/img-post/开发/NAS/蜗牛星际-黑群晖/利用群晖VMM虚拟机安装Windows WES7.assets/2019072602041591.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602041591.jpg)[![利用群晖VMM虚拟机安装Windows](利用群晖VMM虚拟机安装Windows WES7.assets/2019072602042321.jpg)](https://www.xgboke.com/wp-content/uploads/2019/07/2019072602042321.jpg)





文章来源：小众分享，作者：lenbs，感谢作者！https://www.smbinn.com/vmminstallwin7.html