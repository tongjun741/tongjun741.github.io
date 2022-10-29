---
tags:
    - 操作系统
    - Windows
---

  [windows10正式版系统](http://www.xitongcheng.com/win10/)自带了Windows Defender，一些用户在使用一段时间后觉得该应用并不好用，因此就想将其彻底关闭掉，这该如何操作呢？其实方法也不难，接下来小编就详细介绍一下操作方法，希望能够帮助到大家。

![Windows Defender安全中心](/img-post/开发/操作系统/Windows/Windows10系统彻底关闭Windows Defender的技巧.assets/1493024632412414345055.jpg)

**一、停用Windows Defender**

不管你用的是哪个版本的Win10，其实只要停了Windows Defender的实时监控，也就等于让这款内置杀软“离职”，至少是被架空了。所以，在创意者更新中，进入Windows Defender安全中心，找到“病毒和威胁防护设置”，把里面“实时保护”等选项关了就好。老版本Win10在设置→更新和安全→Windows Defender中关闭“实时保护”之类的开关就行。

![停用Windows Defender](/img-post/开发/操作系统/Windows/Windows10系统彻底关闭Windows Defender的技巧.assets/1493024632444777759021.jpg)

**二、关闭Windows Defender安全中心**

这个地方要见到老朋友“注册表”了，想想自己都觉得亲切，还记得之前在之家和大家分享各种注册表的时光，还真是无忧无虑……不过今后我还是会继续和大家分享各种自己感兴趣的内容，争取天天见面。

言归正传，关闭Windows Defender安全中心具体方法如下：

1、在Cortana搜索栏中输入regedit，按回车键进入注册表编辑器。

2、定位到

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SecurityHealthService

3、在右侧找到Start。

![关闭Windows Defender安全中心](/img-post/开发/操作系统/Windows/Windows10系统彻底关闭Windows Defender的技巧.assets/1493024632475489926020.jpg)

4、修改数值数据为4。

5、重启文件资源管理器，或注销再登录/重启系统。



http://www.xitongcheng.com/jiaocheng/win10_article_32912.html