---
tags:
    - 操作系统
    - Windows
    - WSL
---

Windows 10 2004版本中更新到WSL2之后发现和之前一直用的Proxifier存在冲突，启动WSL的时候会出现提示：

参考的对象类型不支持尝试的操作。
1
想要用WSL2的话就得执行

netsh winsock reset
1
但是再运行Proxifier的时候会出现提示:


自动修复完成后Proxifier可以正常用了，但是WSL2再启动就又回到之前的错误提示，好烦~
逛了下WSL在Github上的Issue,发现了这个:

https://github.com/microsoft/WSL/issues/4177#issuecomment-597736482

We have a tool that can make this call:
http://www.proxifier.com/tmp/Test20200228/NoLsp.exe
Please just run as admin with the full path to wsl.exe as the parameter:
NoLsp.exe c:\windows\system32\wsl.exe

上面给出的工具下载地址直接打不开（你懂的），传了一份放到CSDN上:
https://download.csdn.net/download/lpwmm/12485013

使用方法就是上面描述的，以管理员身份运行cmd后执行

NoLsp.exe c:\windows\system32\wsl.exe
1
然后再执行wsl启动就ojbk了~再同时启动Proxifier也没有报错了!

————————————————
版权声明：本文为CSDN博主「DexterLien」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/lpwmm/article/details/106473251