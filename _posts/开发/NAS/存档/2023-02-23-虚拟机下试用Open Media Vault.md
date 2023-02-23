---
tags:
    - NAS
    - 存档
---

虚拟机下试用Open Media Vault

iso 下载地址： https://sourceforge.net/projects/openmediavault/files/



说明

最近一直在研究nas系统，试了好多，目前家里是seafile，不过家里用文件被打碎了太麻烦，在centos上弄nextcloud也不是很方便，因为我linux属于小白，然后想到了有Open Media Vault，这个系统是基于debian内核搞的，自带的功能很强大了，还有很多插件，原来2.X的版本里面有集成了owncloud插件，不过现在3.X的版本owncloud插件没集成进去，不过看到能自己安装，所以来测试下这个系统。另外由于他的开发人员原来也是开发freenas的所以他自己直接有web管理界面，里面开samba，做软raid都很方便，

官网：https://www.openmediavault.org/

我准备弄个8G的u盘，安装系统，然后另外在插4块硬盘做软raid，保存自己需要的数据。

实验步骤：安装OMV系统，开通samba，安装OMV插件，安装nextcloud

安装OMV系统

我们先创建这样一台虚拟机

这个是测试而已，实际使用请大家自己搞台电脑出来，像我是4块2T做radi5 你预算少可以准备2块硬盘，做个raid1，这样资料安全有保证

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/76051cc5e55b4f6dbbd31f2a7247baad.png)

进行安装

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/075b50780d934b3aa4b48004b565f973.png)

选择中文安装，放心不像Ubuntu 16.04 安装的时候用中文还会报错，这个中文挺稳定的

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/8d2847c804064b42850f54c46079b1b1.png)

是

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/14f7465df1e34214a4afdd4f1a53cef3.png)

大陆就选中国吧

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/00be3650782c4c9b96fdfbaf1c87fc69.png)

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/cc3e382da7514ade9bb578e7cb450977.png)

开始加载安装组件

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/5d40c36a59b64295900aebfa3bb64971.png)

主机名我就用默认了

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/e8dc83dfc10f4ce7bdb16590c4503745.png)

这里也默认了

![](//note.youdao.com/src/70094374F900409BAC4D226C572A9D96)

输入系统底层的密码，不是web界面的密码

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/aca30e86cdd54dd4a0fed860e68fc05e.png)

读取时间

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/91330bbd799543e485358412efaf16aa.png)

系统装到8G的U盘里

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/87b5146b9056471294c0751dddf20d03.png)

开始安装系统，安装到一半会卡住，别急等一会就好

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/219f8ef849b347c3a9fb301b741942fd.png)

设置镜像

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/f214f878a1be448aabb5092080a741a3.png)

我就选163了，随便你

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/7a2898a621b64e268f3c64a2aca18f04.png)

中间有一步是配置代理，我手快了没截图就按回车了，不过我想家里应该没人吃饱了去用代理上网吧

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/67f55a9545fd45a9b638ee410d6f7698.png)

安装grub引导

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/fa672bc4b7254ce7b1d0890ae4bb78d6.png)

放sda中，因为这个就是u盘

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/2087b3ae3ea941f7a5aa5fd1792a14f3.png)

安装完成重启

![](/img-post/开发/NAS/存档/虚拟机下试用Open Media Vault.assets/3668b2595665416b921c96de69a84758.png)

安装完成后，我们可以在机器界面上看到已经有ip地址192.168.2.120 下面一行显示登陆网页用户名admin 密码openmediavault。后面就全部用网页来管理了。

![](//note.youdao.com/src/6B0978F0D5C64CF2823E65F1AB3F9AC4)

配置OMV系统

我们登陆OMV，输入密码就可以进入主界面

![](//note.youdao.com/src/27ED7839E122415DBDFBF37BF678AF67)

这里就是主界面，很干净方便

![](//note.youdao.com/src/C04B8934EC9444089FF4F4552068BE20)

更新系统

我们打开更新管理，先更新下系统，打下补丁，以后有更新都在这里可以看到

![](//note.youdao.com/src/EF81FB7EB8694D4B9ACEF425B31A9B0E)

等更新完成后就可以点击关闭，更新过程中无法关闭

![](//note.youdao.com/src/EE0205AF6BD447038F5990223AA2CD27)

安装插件

插件使用第三方的源，也是他们官方认证的

http://omv-extras.org/joomla/index.php/guides

我们登陆网站在下面有deb包，我们下载后上传到插件页面安装下就能看到其他的插件

![](//note.youdao.com/src/A2C4482D59B744E881DF5992B552C607)

上传插件

![](//note.youdao.com/src/F6256227EFC441FAA55657D3FB01142E)

安装

![](//note.youdao.com/src/A2D19319A24F4FE9A74304EE1DF0EDFF)

安装完成后，会自动刷新页面，就会发现插件多了很多

![](//note.youdao.com/src/EA6D577198614106BEE32127473A27B5)

设置系统时间

我们在时间和日期中设置下时区，然后保存下，让时间自动从网络同步

![](//note.youdao.com/src/103CADB6C81E4425847FF18E1991C1B9)

常规设置

在这个设置中，我们可以修改OMV的web端口，网页超时时间，默认5分钟太短了，我设置了长一点

![](//note.youdao.com/src/70ECBCA0A8B74F2D9C383277374A08BA)

硬盘管理

S.M.A.R.T.

开始SMART，这样硬盘出问题能自动检测

![](//note.youdao.com/src/7545368B636F4E2EAD0B7C37488E17DC)

这里可以看到硬盘的型号序列号，如果那块硬盘有问题，找起来也方便

![](//note.youdao.com/src/C4C3E843B7224DC181E7288029157B2B)

RAID管理

我们进入RAID管理，然后选下4块盘，设置下RADI 5，名字叫MD0

linux下面都是用md这个来命名的，所以我也这么叫了

![](//note.youdao.com/src/E8E807E5F08140B4BAE71BD41CCA06A2)

创建完成后会进行初始化，下面有初始化的百分比，等初始化完成后在创建文件结构。

![](//note.youdao.com/src/0102080FD86D4D1B84DF6E4608099AFE)

文件系统

我们进入文件系统，选择创建，选下我们刚才创建的RAID，然后输下名字继续叫MD0，然后文件系统叫EXT4

![](//note.youdao.com/src/A74801EE13184922AE55F22C2AA4C3C0)

选是后会提示你，要被格式化了

![](//note.youdao.com/src/88BECDF21B8B40549F5DA877BAE2AAB9)

![](//note.youdao.com/src/3F2867AF143B4DFB9655CE3C58CF1E3D)

这时文件系统还是不能用的，我们要选择下挂载

![](//note.youdao.com/src/C4C7FCC23899434EBD2ADFB16CC7E927)

挂载完成

![](//note.youdao.com/src/92E9663AF41F41FCB7281C27ABF763B3)

设置SAMBA共享

我家里的NAS我准备建立一个SAMBA的用户，我自己用，然后共享文件3个一个叫NAS，包括NextCloud都装在里面，这样我可以方便的直接上去管理，然后一个叫Download的文件夹，脱机下载插件下载的文件就放里面，然后一个叫Software，我自己的软件都在里面，其他包括我老婆都不用SAMBA，都用NextCloud上面，我尽量让自己的NextCloud上保存的都是轻量级的文件，大容量的就放在SAMBA上。

你自己想如何设置还是每个人不同的，不用照我的来。

建立用户

我们先建立一个用户，用于SAMBA共享

![](//note.youdao.com/src/348EBB8FCCF04627A97E0211AE25E869)

![](//note.youdao.com/src/E536195C46F3439DB21CA8AAE4F6BE4C)

建立共享文件夹

![](//note.youdao.com/src/08BC3722FD31445794102537E608604A)

![](//note.youdao.com/src/8CA17DE61085409B8AF8C6BFA78B3E38)

对每个文件夹添加下用户权限

![](//note.youdao.com/src/6DC47A6ACA81409FBF393B92CEDE278B)

设置SAMBA

我们开启下SAMBA，不知道的选项就不用改了

![](//note.youdao.com/src/A73AD5AC1CAA493E82214B05BF00288D)

添加共享文件夹

![](//note.youdao.com/src/6FA5B1F11DBF4C69822DCA83B3656761)

![](//note.youdao.com/src/273255CD81494EB3B95DF9035833B82A)

设置好后我发现访问共享不行，然后重启了下服务器ok了，都能看到了，可能是因为我是虚拟机的关系，我朋友直接设置好就可以使用了。我们可以进去然后新建下文件，看看权限是否正确

![](//note.youdao.com/src/1EB01E0DD2CC4F00A646CDD4D6B99B64)

https://www.azurew.com/6350.html

