---
tags:
    - 其它
---

https://zhuanlan.zhihu.com/p/55306067



**frp是什么**

frp 是一个可用于内网穿透的高性能的反向代理应用，支持 tcp, udp 协议，为 http 和 https 应用协议提供了额外的能力，且尝试性支持了点对点穿透。关于frp的详细介绍详见[github](https://link.zhihu.com/?target=https%3A//github.com/fatedier/frp/blob/master/README_zh.md)。

## **为什么要内网穿透**

为自由。原谅我一生放荡不羁爱自由（其实我是想早点回家 hhhh）

讲真的，为了远程访问实验室的计算集群，在宿舍，家里，随时随地，都能使用到实验室的高性能机器做仿真和计算。实现的方法很多，如果都是校园网的内部话，直接远程桌面就好了，但是不在同一局域网内，ip地址之间相互ping不通的话，最简单的是使用teamviewer和向日葵，但是这俩个要么花钱要么有商业嫌疑限制，用起来不方便。我们实验室都有用过，teamviewer的用户体验感比向日葵要好，但是用久了就提示有商业行为，虽然我啥也没干，我感觉他在一直监视我在干嘛，细思恐极。。我觉得部署自己的可靠安全的内网穿透方案是必要的。趁着放假在实验室做实验等测试数据的间隙，google了一波方案，说干就干。。。

## **部署步骤**

从frp的架构可以看出frp的工作流程，在服务端部署frps，在要访问的机器上部署frpc，实现服务端对该机器的反向代理。通过访问服务端来实现对该机器的远程访问。

![img](/img-post/开发/其它/windows下基于frp的内网穿透部署.assets/v2-54562948517b17f95898a48b6555bef1_720w.jpg)

**部署环境**

- 公网机器：阿里云的学生机（公网IP）。系统：window server 2012 R2
- 本地机器：高性能电磁仿真工作站集群。系统：window10 pro

在github frp的[releases](https://link.zhihu.com/?target=https%3A//github.com/fatedier/frp/releases)下载最新的代码

修改相关的部署文件即可 好简单的说。。。

**服务端**

修改服务端的部署文件 frps.ini

```as
[common]
bind_addr = 0.0.0.0
bind_port = 7000

dashboard_user = xxxxx #监控的用户名
dashboard_pwd = xxxxxx #监控的密码
dashboard_port = 7500 
```

将frps.ini 及frps.exe 上传到公网机器上，以管理员的身份cmd到其目录下，运行如下的命令

> frps -c frps.ini

看到下图就运行成功了

![img](/img-post/开发/其它/windows下基于frp的内网穿透部署.assets/v2-43761a33014add662515011daee3e382_720w.png)

不用忘记在防火墙将相关的端口打开，不然会报错，或者是访问不了。。。orz

这样服务端就部署成功了，可以访问 dashboard查看监控信息。

![img](/img-post/开发/其它/windows下基于frp的内网穿透部署.assets/v2-a2119afab302fb64c4fac91f42e474a8_720w.jpg)

ps: 服务端的这个命令行窗口不要关，关了服务就挂了。。。。

**客户端**

修改客户端的部署文件 frpc.ini

```text
[common]
server_addr = xx.xx.xx.xx #你的公网机器的ip地址
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 6000
```

将frpc.ini 及frpc.exe 放到本地机器上，以管理员的身份cmd到其目录下，运行如下的命令

> frpc -c frpc.ini

ps: 服务器端对应的端口也要打开，然后这个cmd窗口不能关，不然服务就终止了，这个是下面开机自启动要解决的问题。为了支持远程访问，window下的允许远程访问要打开。

最后就可以愉快的用原生的远程桌面访问机器了。。开森（公网ip:端口）

![img](/img-post/开发/其它/windows下基于frp的内网穿透部署.assets/v2-b088a18cada546312286f0388d11a284_720w.jpg)

**开机自启动**

服务端我没做的开机自启动，是因为的我机器就没有关过，我的任务栏一直保留由很多窗口。但是客户端有做开机自启动的必要了，因为实验室的计算集群是公用的，谁进去手残把这个cmd窗口给关了，我在家哭给他看也没用啊，关键是，虽然我们的集群是24x7x365工作的，但是学校偶尔会停电啊，关机重启是常有的事，每次都运行这些命令不符合window图形界面的用户操作习惯，为了一劳永逸的解决这个问题，我找到了一个开源的开机自启动方案。

duang,duang,duang，就是WINSW。WINSW是什么我就不BB了，自己看github的[readme](https://link.zhihu.com/?target=https%3A//github.com/kohsuke/winsw)吧。我要用他实现window下开机自启动。（ps：linux下开机自启动挺简单的，树莓派上用过很多,用过这个你会发现也挺简单的，强烈推荐）

**使用方法**

- 下载到客户端电脑。[下载地址](https://link.zhihu.com/?target=https%3A//github.com/kohsuke/winsw/releases)
- 放到和frp相同的文件夹下面，改个短小的名字 winsw.exe. 因为我懒得在命令行里面敲东西
- 新建一个winsw.xml。内容如下 详细的[配置参见](https://link.zhihu.com/?target=https%3A//github.com/kohsuke/winsw/blob/master/doc/xmlConfigFile.md)

```abap
<service>
    <id>frp</id>
    <name>frp</name>
    <description>frp remote control</description>
    <executable>frpc</executable>
    <arguments>-c frpc.ini</arguments>
    <logmode>reset</logmode>
</service
```

- 然后以管理员的身份cmd到该文件夹下，执行如下命令

> winsw install
> winsw start

开机自启动就大功告成了。我用自己的老电脑亲测可用。

## 最后

我把这个推广到了整个天线与射频组。告诉师兄们如何有序的使用集群资源，就是不要同时连接机器，因为window10 pro不支持多用户同时登陆，要是win server或者是linux就好了。就是使用机器前看下bashboard,当前有连接的情况下不要使用，稍等片刻。当然大家也不要霸占资源。不要在集群上建模和写代码，只能仿真和跑算法，偶尔登陆查了下结果就好了。

![img](/img-post/开发/其它/windows下基于frp的内网穿透部署.assets/v2-f741fa0cc02500371d4e39a47bd352d9_720w.jpg)

------

先感谢下马爸爸的阿里云，学生机真香，虽然40G的硬盘捉襟见肘，10Mpbs的网络带宽很捉急，就一个月1000G的流量我很满足。我开始担心网络延迟很严重，部署了用不了，但是结果真的出乎我的意料的好。笔芯阿里云。

然后是感谢github,全球最大的男性交友网站。我在这学到了很多，找到了很多的矿，站在巨人的肩膀上摘苹果，不造重复的轮子。我也会努力贡献我的code,做社区的一员。

最后感谢我的老板，学识渊博的Buris。不远万里来到中国，60岁的人了，再可以退休享乐的年纪还在折腾，我有什么理由在最该奋斗的年纪选择安逸呢。这半年学到了很多东西，给了我很多学习，工作，科研上的指导，挺崇拜他的，祝愿身体健康，在中国工作愉快。

最近趁着放假，呆在实验室里面写总结和反思，看过的paper和实验的数据，我担心我过完年回来忘记了。开始写的第一篇知乎文章，就当我胡言乱语，不知所云好了。

![img](/img-post/开发/其它/windows下基于frp的内网穿透部署.assets/v2-4d7c579c032d16fd9685f730f79287f6_720w.jpg)



编辑于 2019-07-20