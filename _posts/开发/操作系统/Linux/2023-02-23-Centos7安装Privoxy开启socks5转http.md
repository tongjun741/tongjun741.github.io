---
tags:
    - 操作系统
    - Linux
---

Centos7安装Privoxy开启socks5转http

下午很清闲，所以有空折腾了一下privoxy，系统是centos7，目的是将socks5转http，因为好多应用场景使用socks5不方便，种种资料显示privoxy是最佳方案，而且用的人多，自然遇到问题查找相关帮助就容易很多；本测试均在本地电脑操作，非远程vps主机上！！！

 

安装命令:yum install -y privoxy

升级命令：yum update privoxy -y

 

执行上面命令的时候竟然失败，换了阿里云和163网易的源也一样，最后换了清华大学的镜像源成功安装，不知道哪个地方出了问题；清华大学centos镜像源地址：http://mirrors.itzmx.com/help/centos/

 

Privoxy配置文件路径为：/etc/privoxy/config

config里面好大一段内容，通通都是注释，讲解各种牛叉功能的介绍，可以一窝蜂删除掉看着头大并且眼花；

增加以下内容：

listen-address :8119

forward-socks5 / 192.168.10.1:7001 .

 

说明：8119是http端口，192.168.10.1:7001这个是已经存在的socks代理，末尾处有小数点不能遗漏；

 

控制命令：service privoxy start（stop/restart）

如果没有出现提示那么就正常，如果有额外英文显示说明不正常，之前我因为8118端口已经被占用导致运行失败，所以换成了8119；

 

测试http代理是否正常方法（返回HTML代码就说明木有问题）：

curl --connect-timeout 2 -x 127.0.0.1:8119 http://google.com

以上就是privoxy基本安装和使用的基础方法；刚刚在手机上测试了一下上YouTube看新闻联播很爽，速度从未有过的丝滑般流畅；友情提醒：不要盲目尝试，上网有风险，手痒需谨慎；





https://www.5yun.org/14197.html

