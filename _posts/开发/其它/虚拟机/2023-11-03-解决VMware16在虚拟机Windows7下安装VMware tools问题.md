---
tags:
    - 其它
    - 虚拟机
---

问题描述：

Windows7虚拟机简易安装完成后，“安装VMware Tools”选项为灰色，无法点击安装。

解决方法：

关闭虚拟机Windows7，查看虚拟机设置，将CD 和软盘改成自动检测。再次进入虚拟机Windows7，“安装VMware Tools”恢复正常。

![img](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/202211301527517.png)

问题2：安装过程提示驱动签名问题

- 问题描述：

- 安装VMware Tools过程中，弹出“Windows 无法验证此驱动程序软件的发布者”。
  点击“始终安装”后，弹出错误提示“安装程序无法自动安装 Virtual Machine Communication Interface Sockets(VSock)驱动程序。必须手动安装此驱动程序”
  最终导致安装失败：

- ![img](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/202211301527518.png)

- 解决方法：
  由于微软更新了驱动程序签名算法，2019年开始弃用SHA1，改用SHA2。猜测VMware Tools驱动程序使用SHA2，而Windows7只支持SHA1，需要下载安装补丁kb4474419来支持SHA2算法。

- 下载地址：[Microsoft Update Catalog](https://www.catalog.update.microsoft.com/Search.aspx?q=kb4474419)

- 自行安装对应的版本补丁

- ![img](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/202211301527519.png)

- 安装后需先重启后安装VMware Tools

- 这时候没有安装tools补丁无法拷贝到虚拟机,可以使用samba共享出来给虚拟使用,或者放在u盘,在虚拟启动之前设置一个挂载.

- win10共享文件夹方式可以参考我的另一篇文章

## Mware tools选项为灰色的解决方法二

打开虚拟机设置，然后查看硬件设备中是否有**软盘**设备。

![img](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/202211301520201.png)

如果有，需要关闭虚拟机后移除软盘设备后再尝试安装VMware tools。

VMware tools安装失败的解决方法：

安装过程如果出现下面情况：

![img](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/202211301520202.png)

![img](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/202211301520203.png)

### 无法安装原因：

win7缺少kb4474419补丁，造成VMware tools安装失败。下载安装kb4474419补丁尝试解决。

附上下载链接：[Microsoft Update Catalog](https://www.catalog.update.microsoft.com/Search.aspx?q=4474419)（**为了方便安装推荐下面的方法**）。根据自己电脑系统位数下载安全更新。

**比较方便的安装方法：**

由于虚拟机无法安装VMware tools增强工具，在实体机下载的补丁文件无法移动到虚拟机，有一种比较方便的方法是**将补丁文件打包成ISO文件，通过VMware软件加载：**（制作方法可以自己搜索，这里不介绍了）

已经制作好的ISO文件：

百度网盘（推荐）

链接: https://pan.baidu.com/s/17Mv7Tz6OZKvHk3vQfQLCuw 提取码: 6skh 

--来自百度网盘超级会员v9的分享

 [阿里云盘](https://www.aliyundrive.com/s/ELuYMz4oLVg)（**注意：**由于阿里云盘分享不支持该类型，所以下载后一定要将原来的后缀名"**.png"改成".iso"**）

**以下为虚拟机加载iso文件教程：**

![img](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/202211301520214.png) 

![img](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/202211301520215.png)

 ![img](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/202211301520216.png)

这样在虚拟机上安装补丁软件就比较方便了。 

## 最终结合多个教程总结出解决方法

点击安装vmware tools后，出现的错误提示弹窗：

![在这里插入图片描述](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/2022113015344210.jpg)

## 一、将操作系统更新到SP1

百度经验：Win7虚拟机无法安装vmware tools怎么办？

这篇已经指出了根本问题：Win7虚拟机安装vmware tools提示安装程序无法进行，需要将操作系统更新到SP1。

## 二、自动更新问题

但是根据教程打开自动更新时却遇到新的问题：win7 windows update 无法更新，错误代码80072EFE

这个问题蛮恶心人的，新装的Win7，Windows Update完全无法使用，所有以前推出的更新都无法安装

2021年新安装的Win7系统，Windows Update无法更新，提示错误代码80072EFE

https://www.jb51.net/os/windows/857038.html

## 三、解决方案

主要结合以上两个解决方法，如果后续工作不必完全修复自动更新功能，只需要打好SP1补丁即可

最好先安装一下Windows6.1-KB3020369
这个补丁我也不确定是否必须安装，我先安装这个补丁后才进行接下来的安装

### 1.安装补丁KB3020369后安装KB976932

安装SP1补丁包windows6.1-KB976932
这里找了很多地方终于找到了正确的下载链接：

https://www.catalog.update.microsoft.com/Search.aspx?q=kb976932

根据自己电脑是64位或者32位选择对应
链接，打开后注意第二个链接是976932![在这里插入图片描述](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/2022113015344211.png)

### 2.安装补丁KB947821

下载完毕后安装又出现问题，会被阻止安装，按照提示进入
https://docs.microsoft.com/zh-CN/troubleshoot/windows-server/deployment/fix-windows-update-errors#resolution-for-windows-7-and-windows-server-2008-r2

找到适用于 Windows 7 和 Windows Server 2008 R2 的解决方法，进入目录：https://www.catalog.update.microsoft.com/Search.aspx?q=947821

![在这里插入图片描述](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/2022113015344212.png)
我选择了图中高亮的64位版本，进入后有大量链接，
在偏下的部分找到947821补丁下载
![在这里插入图片描述](/img-post/开发/其它/虚拟机/解决VMware16在虚拟机Windows7下安装VMware tools问题.assets/2022113015344213.png)
下载安装完成后重启虚拟机！一定要重启，否则还是无法安装SP1补丁

### 3.安装vmware tools

重启后重试安装补丁包windows6.1-KB976932，此时正常完成安装。最后下载安装vmware tools，可以正常完成安装！



https://www.jb51.net/softjc/857040.html