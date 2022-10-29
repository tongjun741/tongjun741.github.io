---
tags:
    - 操作系统
    - Mac
    - ParallelsDesktop
---

  ![WX20210419-145837@2x.png](/img-post/开发/操作系统/Mac/ParallelsDesktop/Parallels Desktop 16.1.2 完美破解版下载.assets/2021041954044205.png)

  自从MacBook Pro升级了Big Sur之后，Parallels Desktop也随之升级到了16，然而一开始并没有完美的破解版本，需要在鱼与熊掌之间做出选择，最先被大神破解的Parallels Desktop，虚拟机选择可以上网，就必须牺牲USB识别的功能，反之，如果选择了USB的功能，就必须牺牲掉上网的功能，实在令人头疼。

  官网个人新用户498一年，企业用户一年698，一年后要继续使用就需要续费；个人永久授权698，但无法升级，以后一旦MacOS有重大版本升级，有可能就无法继续使用了。

  好在TNT大神近日终于弄出来了16.1.2的破解版，只需稍作修改，即可兼得上网和USB两个关键功能，需要学习研究的可在文末链接下载，正式用途请购买正版。

![WX20210419-151635@2x.png](/img-post/开发/操作系统/Mac/ParallelsDesktop/Parallels Desktop 16.1.2 完美破解版下载.assets/2021041955289757.png)



**安装时如提示有新版本，请忽略，仍然安装当前版本，安装之后关闭自动更新。**  

**如果遇到网络初始化失败的问题，请在终端输入以下代码，然后打开虚拟机即可：**

```
sudo -b /Applications/Parallels\ Desktop.app/Contents/MacOS/prl_client_app
```

  网络和USB问题解决方法（更改配置文件的方式,觉得上面每次打开输入命令麻烦的可以采用这种方式）**：**

 **一、网络初始化失败解决方法**

  找到/Library/Preferences/Parallels/network.desktop.xml文件，拷贝出来修改后替换原文件（注意备份）。
修改方法：

```
找到<UseKextless>-1</UseKextless> 改为<UseKextless>0</UseKextless>，保存替代！
```

**二、USB不识别问题\**解决方法\****

  找到/Library/Preferences/Parallels/dispatcher.desktop.xml文件，拷贝出来修改后替换原文件（注意备份）。
修改方法：

```
找到<Usb>0</Usb> 改为<Usb>1</Usb>，保存替代！
```

  **如果配置文件里没有，可以自己粘贴上去（注意格式）****。**

  **注意：修改配置文件时请完全关闭PD，配置好之后重新打开PD并重启虚拟机系统才会生效。**

  另外虚拟机系统锁屏和开屏的时候，有可能会跳出D版提示，可以直接关掉不加理会就好，不影响正常使用，接受不了的可以选择支持正版。



**温馨提示：经过试用，U盘移动硬盘可以正常使用，但是银行U盾之类的有可能不能用。**



…………………………………………………………………………………………我是分割线……………………………………………………………………………………

**PS:很多朋友安装和使用过程遇到了不少问题，可以在评论区留言。博主能力范围的一定为您解答，下面整理了一下常见问题解决方法。**

**一、找不到<UseKextless>标签。**

**解决方法：找到/Library/Preferences/Parallels/network.desktop.xml文件，复制到桌面，用文本编辑器打开，直接在文件开头几行的<IPv6Enabled>标签下面新建一行，输入<UseKextless>0</UseKextless>即可。USB不识别解决方法一样。**

**![net.png](/img-post/开发/操作系统/Mac/ParallelsDesktop/Parallels Desktop 16.1.2 完美破解版下载.assets/2021062146652589.png)**

**
**

**二、虚拟机系统麦克风不识别问题。**

**解决方法：关闭虚拟机系统，然后右键点PD图标，控制中心，在虚拟机系统列表点击小齿轮设置，硬件，音频和摄像头，麦克风改为默认或者选择mac麦克风即可。**

![mkf.png](/img-post/开发/操作系统/Mac/ParallelsDesktop/Parallels Desktop 16.1.2 完美破解版下载.assets/2021062147098993.png)

下载链接：

链接: https://pan.baidu.com/s/1Sh3i_eBnVWDxaUe-HivjUg 提取码: 28p1



如果你是Apple M1芯片，请点击链接下载针对M1芯片的版本：

链接: https://pan.baidu.com/s/1UCbSNdIHwSc24ZGcAoqdzA 提取码: v1dw





http://www.soulsky.net/?mod=pad&act=view&id=102