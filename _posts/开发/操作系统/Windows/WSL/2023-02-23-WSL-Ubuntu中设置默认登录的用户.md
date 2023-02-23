---
tags:
    - 操作系统
    - Windows
    - WSL
---

## WSL如何切换ROOT默认登陆

心血来潮想在电脑上用WSL跑一个MYSQL，就装了个Ubuntu在电脑上，当然众所周知的是这个Ubuntu默认是普通用户登陆的。

Mysql登陆的时候会遇到权限配置的问题，心想很麻烦，就考虑打开Ubuntu的时候直接切成ROOT，后来找到了一个办法。

**WIN+X，管理模式打开Powershell，输入如下命令：**

*ubuntu config --default-user root*  
 之后再打开WSL，默认就是ROOT用户登陆了。







作者：花讽院_和狆
链接：https://www.jianshu.com/p/a41ea800fdb8
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。