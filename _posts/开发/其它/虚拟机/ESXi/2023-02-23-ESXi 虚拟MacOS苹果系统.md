---
tags:
    - 其它
    - 虚拟机
    - ESXi
---

ESXi 6.7U3上可以非常方便的安装MacOS进行测试，这里做一下几个关键的步骤。

### 1、制作cdr/iso镜像

#### 1.1 首先从用apple官网下载原始文件：

```
macOS Catalina 10.15
https://support.apple.com/zh-cn/HT201475
macOS Mojave 10.14
https://support.apple.com/zh-cn/HT210190
macOS High Sierra 10.13
https://support.apple.com/zh-cn/HT208969
macOS Sierra 10.12.
https://support.apple.com/zh-cn/HT208202
OS X El Capitan 10.11
https://support.apple.com/zh-cn/HT206886
```

或者从其他地方下载原版dmg文件，然后需要找一个macOS系统来制作cdr文件



## Python脚本下载

**开源地址**：**[macadmin-scripts](https://github.com/munki/macadmin-scripts)**

下载[installinstallmacos.py](https://github.com/munki/macadmin-scripts/blob/master/installinstallmacos.py)到Mac，然后用python运行，如图：

[![《macOS完整安装包下载方法》](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/QQ20190722-121342@2x.png)](https://www.newlearner.site/“wp-content/uploads”/2019/07/QQ20190722-121342@2x.png)

该脚本的原理是爬取苹果官网下载链接并收集所有可供下载的os，目前可供下载的os如下：

![image-20201118210208147](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/image-20201118210208147.png)

选择你要下载的镜像并回车即可。该脚本下载使用的链接经过抓包也是官网链接，放心使用。

**下载完毕不要忘记[校验SHA1值](https://www.newlearner.site/?p=1794&preview=true#SHA1)**

#### 1.2 创建一个dmg空文件

10.13需要5G，10.14需要6G，10.15需要8G，这里以10.14为例
然后依次为格式参数、文件系统格式
因为内存够大，所以直接挂在tmp目录，这样速度更快，如果内存不大可以自己选择文件路径

```
hdiutil create -o /tmp/Mojave.cdr -size 4g -layout SPUD -fs HFS+J
```

#### 1.3 挂载到虚拟磁盘

挂载上面新建的 dmg 镜像到虚拟磁盘，载点为 install_build，之后会使用，需要对应

```
hdiutil attach /tmp/Mojave.cdr.dmg -noverify -mountpoint /Volumes/install_build
```

#### 1.4 将下载的系统安装文件写入虚拟磁盘

将所下载的系统安装app文件写入到上面挂载的虚拟光驱磁盘中，即我们第一步建立的空镜像，首先需要输入管理员密码，然后回车，之后等待执行结束，包括擦除磁盘、复制文件、添加启动，结束之后，桌面上之前显示 untitled 的虚拟磁盘会变成我们需要的系统名称
如果是下载dmg文件，就将dmg挂载后，先把app文件复制到**应用程序**目录，然后将dmg取消挂载

```
sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/install_build
```

#### 1.5 取消挂载建立的dmg镜像

取消挂载建立的dmg镜像，方便后续编辑，载点名已经从原来的install_build更改为Install macOS Mojave

```
hdiutil detach /Volumes/Install\ macOS\ Mojave
```

#### 1.6 格式转换

将制作好的dmg文件转换为cdr

```
mv /tmp/Mojave.cdr.dmg ~/Desktop/Mojave.dmg
hdiutil convert ~/Desktop/Mojave.dmg -format UDTO -o ~/Desktop/Mojave.iso
```

#### 1.7 删除dmg镜像，重命名文件

删除1.2建立的 dmg 镜像，释放磁盘空间
如果想保留，就不用删除

```
rm ~/Desktop/Mojave.dmg
```

将cdr文件重命名为iso
实际上macos下的光盘镜像cdr格式就相当于Windows下常见的光盘镜像iso格式
在esxi中也可以直接挂载cdr文件

```
mv ~/Desktop/Mojave.iso.cdr ~/Desktop/Mojave.iso
```

其他版本系统，修改相应名称即可

### 2、ESXi补丁

这个补丁原作者的github已经没了，这里分享一个：
esxi-unlocker-300.tgz: https://n459.com/file/20182514-448752132

如果不打补丁，在ESXi中创建虚拟机后，挂载iso会无限重启！

打补丁方法和前一篇文章”ESXi NVME 启动降级方法”类似，打开ssh，然后scp进去，再ssh登录执行即可。

[![img](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/1592141738733-20201118210000263.png)](https://raw.githubusercontent.com/white-alone/blog_img/master/ESXi_虚拟MacOS苹果系统.md/1592141738733.png)

[![img](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/1592141950990-20201118210000349.png)](https://raw.githubusercontent.com/white-alone/blog_img/master/ESXi_虚拟MacOS苹果系统.md/1592141950990.png)

之后重启ESXi即可

### 3、创建macOS虚拟机

之后就可以直接创建macOS虚拟机了，10.15用10.14的即可

[![img](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/1592142052687-20201118210000368.png)](https://raw.githubusercontent.com/white-alone/blog_img/master/ESXi_虚拟MacOS苹果系统.md/1592142052687.png)

挂载cdr或者iso文件，显存最好设置大一些

[![img](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/1592142182647-20201118210000063.png)](https://raw.githubusercontent.com/white-alone/blog_img/master/ESXi_虚拟MacOS苹果系统.md/1592142182647.png)

[![img](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/1592142407468-20201118210000666.png)](https://raw.githubusercontent.com/white-alone/blog_img/master/ESXi_虚拟MacOS苹果系统.md/1592142407468.png)

安装10.14及以下版本时候需要注意，因为有证书过期的问题，会报错

[![img](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/1592142456969-20201118210000007.png)](https://raw.githubusercontent.com/white-alone/blog_img/master/ESXi_虚拟MacOS苹果系统.md/1592142456969.png)

需要编辑下虚拟机设置，断开虚拟机网络，再利用”实用工具-终端”将时间设置到2016年2月14日之前，比如2015年

[![img](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/1592142509569.png)](https://raw.githubusercontent.com/white-alone/blog_img/master/ESXi_虚拟MacOS苹果系统.md/1592142509569.png)

之后退出终端再安装就行

### 4、VMware Tools

安装完系统后，还需要安装VMware Tools增强工具，但ESXi默认没有附带macOS的。

当前vmware tools最新版本 11.1.0：
https://my.vmware.com/web/vmware/details?downloadGroup=VMTOOLS1110&productId=974

[![img](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/1592142894263.png)](https://raw.githubusercontent.com/white-alone/blog_img/master/ESXi_虚拟MacOS苹果系统.md/1592142894263.png)

这里也分享一个：
VMware-Tools-darwin-11.1.0-16036546.zip: https://n459.com/file/20182514-448752130

解压缩将iso文件传到ESXi上，然后直接挂载安装即可

[![img](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/1592143042743.png)](https://raw.githubusercontent.com/white-alone/blog_img/master/ESXi_虚拟MacOS苹果系统.md/1592143042743.png)

安装完成，即可享用了。可以通过vCenter制作成模板，方便部署安装

[![img](/img-post/开发/其它/虚拟机/ESXi/ESXi 虚拟MacOS苹果系统.assets/1592143186063.png)](https://raw.githubusercontent.com/white-alone/blog_img/master/ESXi_虚拟MacOS苹果系统.md/1592143186063.png)





https://www.white-alone.com/ESXi%20%E8%99%9A%E6%8B%9FMacOS%E8%8B%B9%E6%9E%9C%E7%B3%BB%E7%BB%9F/

https://www.newlearner.site/?p=1794&preview=true#SHA1