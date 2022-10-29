---
tags:
    - 其它
---

解决sublimeText3无法安装插件问题 -- There are no packages available for installation

尝试了n种，终于找到了终极解决方法（时间紧急的请拉到最后看）



1.CSDN提供的解决方法



之前用这个方法解决了，但现在也不行了



2.提供三个解决方法



使用方法二：







还是不行，出现下面错误（按Ctrl+`） 







搜索出现了下面的解决方法，但依然失败！ ！！



加载MNIST报错：[WinError 10060] 由于连接方在一段时间后没有正确答复解决办法



3.在我想要放弃的最后一刻终于找到了解决方法



解决sublime text无法安装插件问题——简书



sublime Text 3中安装vue高亮插件以及解决可能出现的问题



解决方法：

Package Control.sublime-settings]修改方法：

Preferences > Package Settings > Package Control > Settings - User

添加



 "channels":

    [

        "http://static.bolin.site/channel_v3.json",

        //"https://packagecontrol.io/channel_v3.json",

        //"https://web.archive.org/web/20160103232808/https://packagecontrol.io/channel_v3.json",

        //"https://gist.githubusercontent.com/nick1m/660ed046a096dae0b0ab/raw/e6e9e23a0bb48b44537f61025fbc359f8d586eb4/channel_v3.json"

    ],



--------------------- 

作者：Ggigi 

来源：CSDN 

原文：https://blog.csdn.net/Jane_3210/article/details/88226906 

版权声明：本文为博主原创文章，转载请附上博文链接！

