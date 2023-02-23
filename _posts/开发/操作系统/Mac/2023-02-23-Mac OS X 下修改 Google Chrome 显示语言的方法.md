---
tags:
    - 操作系统
    - Mac
---

Mac OS X 下修改 Google Chrome 显示语言的方法

当然，如果把系统语言更改为英文，Chrome、QQ 等一系列软件会自动变成英文界面，而且没有提供切换语言的设置。



啪了下 Google，找到了方法，直接在终端运行后重启 Chrome 即可更改。



英文 -> 简体中文



defaults write com.google.Chrome AppleLanguages '(zh-CN)'

1

简体中文 -> 英文



defaults write com.google.Chrome AppleLanguages '(en-US)'

1

英文优先，简体中文第二。反之改一下顺序



defaults write com.google.Chrome AppleLanguages "(en-US,zh-CN)"

--------------------- 

作者：至天 

来源：CSDN 

原文：https://blog.csdn.net/maxsky/article/details/52411977 

版权声明：本文为博主原创文章，转载请附上博文链接！







