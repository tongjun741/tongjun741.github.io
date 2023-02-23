---
tags:
    - 操作系统
    - Linux
    - SSH
---

在~/.ssh/config文件第一行增加以下内容：

```
AddressFamily inet
```



如果是仅使用ipv6可以这样写：

```
AddressFamily inet6
```



也可以为每个主机单独定义。



https://www.putorius.net/force-ssh-client-to-use-ipv4-or-ipv6.html

https://blog.csdn.net/senlin1202/article/details/122081089