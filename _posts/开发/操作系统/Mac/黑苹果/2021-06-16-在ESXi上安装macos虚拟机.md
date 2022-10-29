---
tags:
    - 操作系统
    - Mac
    - 黑苹果
---

在ESXi上安装macos虚拟机

在昨天之前，我没想过苹果的操作系统居然可以不运行在苹果的硬件之上。

再一想，是AIX什么时候可以运行在ESXi上？也许永远没有机会了，毕竟已经进入云计算大数据时代了。

安装并没什么复杂的，对最新版的macos10.14，只要下载3个软件就可以了：

1）下载VMware Unlocker 2.1.1 （for ESXi 6.7/6.5U2）

https://drive.google.com/file/d/117EY6n8lq5\_\_yroE5qIFqJZxMG0ZF\_2b/view?usp=sharing

```
# ESXI7 需要使用以下命令解锁：
/bin/sh  ./esxi-install.sh
```





2）下载MacOS 10.14 Mojave

https://drive.google.com/file/d/1tCqH1rkw9YXOs--UXcY5RmsE_RRuXJYx/view?usp=sharing

3）下载VM Tools for macos（darwin.iso）：

https://bit.ly/VMToolsforMAC

嗯，我还找文骏同学弄了个anyconnect下载这些软件。

唯一要说的是，去vmware上看看支持矩阵，不然可能就是瞎折腾。

https://www.vmware.com/resources/compatibility/search.php?deviceCategory=software&details=1&releases=408&operatingSystems=248&productNames=15&page=1&display_interval=10&sortColumn=Partner&sortOrder=Asc&testConfig=16

够300字了，贴个图吧

![](https://ask.qcloudimg.com/http-save/yehe-3382649/7fzuin2gns.jpeg?imageView2/2/w/1620)

看到这个矩阵了么，这里选择的是Guest OS，还有CPU Series、Storage等等其他矩阵关系可以选择。在安装之前，上来看一眼，保不齐可以节省一两天的时间。

对于macos来说，ESXi6.5只能安装10.12版本的莫哈维沙漠(mojave).

最新版的10.15版本的远程轰炸机(catalina)要ESXi6.7U3版本才支持。

《天下无贼》里，葛优说二十一世纪最重要的是什么？人才。那什么人叫人才呢？能解决问题的人么？还是能解决掉有问题的人是人才？

大概在去年年中，去见一个客户的时候，VP讲了一句话，说我们还是要问题导向，解决了什么问题？乍一听，非常有道理。

仔细想想，其实这个道理不强，因为问题是无穷尽的。

好多年前看过有本小书叫做《问题背后的问题》，我觉得书名就很说明问题了，这些问题背后有什么问题么？

又回到历史，为什么印度、澳大利亚、新西兰等一众英国殖民地到如今都对大英帝国感恩戴德，现在还有所谓的英联邦的各种活动？而大清之后的越南、琉球、三胖、蒙古等附庸国却几乎是对天朝恨之入骨呢？

核心问题还是思维。

[https://cloud.tencent.com/developer/article/1583551](https://cloud.tencent.com/developer/article/1583551)

[https://blog.csdn.net/weixin_43711537/article/details/100882279](https://blog.csdn.net/weixin_43711537/article/details/100882279)