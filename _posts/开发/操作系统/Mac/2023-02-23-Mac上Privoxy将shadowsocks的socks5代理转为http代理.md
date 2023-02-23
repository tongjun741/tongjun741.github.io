---
tags:
    - 操作系统
    - Mac
---

Mac上Privoxy将shadowsocks的socks5代理转为http代理

http://blog.csdn.net/yanzi1225627/article/details/51064306



背景：shadowsocks虽然用着很爽，但是不是万能的。原因是他是sock5代理，属于局部代理。一些软件根本不支持socks5代理，如AS的gradle，eclipse等均不支持。AndroidStudio本身支持socks5代理，但是那个gradle只支持http代理。另外，还有一个误区，将shadowsocks的全局代理打开就能代理所有请求，这是一种错误的认识。全局和局部，有个前提那就是使用了socks5代理，也即使用了shadowsocks，在此基础上根据pac规则，加上你选择的模式决定是否要用shadowsocks这个梯子。一般而言，我门的浏览器都是默认支持socks5. 对于不支持socks5的，可以使用Privoxy！

一，安装

安装可以通过brew install privacy，也可以访问官网再跳转到source forge上下载：https://sourceforge.net/projects/ijbswa/files/ ，我用的后者进行安装。



二，启动

安装完毕后在/Applications/Privoxy/目录可以看到startPrivoxy.sh 和 stopPrivoxy.sh两个脚本，通过以下两个命令开启或关闭：

打开：sudo /Applications/Privoxy/startPrivoxy.sh

关闭：sudo /Applications/Privoxy/stopPrivoxy.sh



三，配置关联shadowsocks

privoxy 的配置文件在/usr/local/etc/privoxy目录下的config文件，vim config,然后搜索socks5关键字，也即5.2部分找到#        forward-socks5t   /            127.0.0.1:9050 .这一句，这个是整合了tor的，我门在上面加一句即：

 forward-socks5    /               127.0.0.1:1080

	备注：这句话表示将所有请求通过socks5中转，而127.0.0.1:1080正是shadowsocks本地监听的地址和端口。如果你的ss的本地端口不是1080，此处要做对应修改，保持一致。如果你只想在本地电脑使用privoxy，那么痛过这么配置已经ok了。如果你希望局域网里其他终端通过http代理使用privoxy，搜索关键字：listen,最终找到：

listen-address  127.0.0.1:8118

上面这个表示privoxy监听本地的8118端口。将其改为：listen-address  0.0.0.0:8118 即可监听局域网内所有请求。

在iOS上，设置wifi那里的http代理，服务器地址为安装了privoxy的pc的ip地址，端口为8118. 之后就ok了！



四，配置其他软件使用http代理

SublimeText3: 

在Preference－PackageSettings－Package Control－Settings User里，加入这样两句：

"http_proxy": "http://127.0.0.1:8118",

"https_proxy": "http://127.0.0.1:8118",

我的设置如下：

{

	"bootstrapped": true,

	"channels":

	[

		"https://packagecontrol.io/channel_v3.json"

	],

	"debug": true,

	"http_proxy": "http://127.0.0.1:8118",

	"https_proxy": "http://127.0.0.1:8118",

	"in_process_packages":

	[

	],

	"installed_packages":

	[

		"Emmet",

		"Package Control",

		"Pretty JSON"

	],

	"update_check": false

}



Git：

设置http代理：

git config --global https.proxy http://127.0.0.1:8118



git config --global https.proxy https://127.0.0.1:8118

取消http代理：

git config --global --unset http.proxy



git config --global --unset https.proxy



备注：git是可以直接支持socks5代理的，如果只安装了shadowsocks，而不安Privoxy设置方法如下：

git config --global http.proxy 'socks5://127.0.0.1:1080' 

git config --global https.proxy 'socks5://127.0.0.1:1080'



Eclipse及其他软件就不列了。

2016-5-3补充说明：关于git设置http代理，不用使用git config，可以直接在shell里设置，之后在shell里的操作（当然包括git）都会走代理。

[plain] view plain copy

 print?

![在CODE上查看代码片](/img-post/开发/操作系统/Mac/https://code.csdn.net/assets/CODE_ico.png)

![派生到我的代码片](/img-post/开发/操作系统/Mac/https://code.csdn.net/assets/ico_fork.svg)

1. export http_proxy=http://127.0.0.1:8118  

1. export https_proxy=http://127.0.0.1:8118  



五，Privoxy代理所有请求

直接设置wifi的http和https代理即可, Privoxy功能还是很强大的，非局限于本文所述,官网上的一句话：

Privoxy is a non-caching web proxy with advanced filtering capabilities for enhancing privacy, modifying web page data and HTTP headers, controlling access, and removing ads and other obnoxious Internet junk. Privoxy has a flexible configuration and can be customized to suit individual needs and tastes. It has application for both stand-alone systems and multi-user networks.

![](http://img.blog.csdn.net/20160405144657380?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)









