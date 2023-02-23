---
tags:
    - 操作系统
    - Linux
---

在Linux系统中为Snapd设置代理的两种方法

本文将给你介绍在Linux操作系统中为Snapd设置代理的两种方法：更改/etc/environment和覆盖snapd的现有设置，最后附上关于不支持HTTPS的说明。

 

背景

Snap，全称SnapCraft，是一个全新的应用软件环境，在Snap中，软件被封装在类似于Docker的容器中，即开即用，可随时获取，这一切由其后台服务snapd提供支持，从Ubuntu 18.04版本开始，就引入它作为系统的一部分，而其他的Linux发行版（如Deepin）也可以通过软件管理工具进行安装，如sudo apt install snapd。参考在Ubuntu 18.04/Debian上安装和使用Snap的方法。

SnapCraft将软件包分发在自己的服务器上，然而，因为众所周知的原因，访问位于海外的Snap服务器异常缓慢，不加代理的情况下，下载速度会持续降到十几KB每秒，这使得我们不得不想办法通过代理服务器进行加速。

一般地，Linux系统上的一些应用程序会通过读取环境变量http_proxy和https_proxy来应用代理服务器设置，典型的有Chrome，然而Snap比较特别，它不会从环境变量中上述环境变量中读取代理服务器设置，因此直接使用export http_proxy=[代理服务器地址]或export https_proxy=[代理服务器地址]是不起作用的。下面为大家献上解决方法。

 

方法一、更改/etc/environment

/etc/environment是一个Shell脚本，snapd会读取它，应用其中指定的配置信息，因此设置代理服务器的正确目标，实际上就是这里。

在/etc/environment文件中加入以下两行：

http_proxy=http://[服务器地址]:[端口号]

https_proxy=http://[服务器地址]:[端口号]

然后重启snapd服务，运行以下命令：

sudo systemctl restart snapd

注：snapd读取/etc/environment，因此设置代理环境变量是有效的，在Ubuntu上通过设置→网络→网络代理自动完成的，因此只要在更改该文件后重新启动snapd就可以了。

 

方法二、覆盖snapd的现有设置

除了修改environment文件，也可以修改snapd服务的配置文件，在其加入Environment信息，信息内容实际上就是方法一中设置代理服务器的语句。

运行以下命令，打开snapd的配置文件：

sudo systemctl edit snapd.service

在打开的文本编辑器中，加入以下代码，共三行：

[Service]

Environment=http_proxy=http://proxy:port

Environment=https_proxy=http://proxy:port

最后重新加载snapd服务，运行以下命令：

sudo systemctl daemon-reload

sudo systemctl restart snapd.service

至此，Snap安装现在应该通过指定的代理了。

 

关于不支持HTTPS的说明

一般的本地代理都不支持HTTPS，所以https_proxy的值也只能是http地址，比如在Ubuntu 18.04虚拟机上安装Conjure-Up，运行sudo snap install conjure-up --classic命令会出现如下错误：

cannot install "conjure-up": Post https://api.snapcraft.io/v2/snaps/refresh: proxyconnect tcp: EOF

注：知道有这个问题存在即可。

 

