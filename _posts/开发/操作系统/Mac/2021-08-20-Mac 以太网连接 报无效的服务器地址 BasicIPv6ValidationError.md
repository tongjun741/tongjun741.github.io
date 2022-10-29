---
tags:
    - 操作系统
    - Mac
---

在mac系统下(os x 10),手动设置ip地址后，弹出错误提示“无效的服务器地址 BasicIPv6ValidationError”
 解决的办法是:

```
networksetup -listallnetworkservices  //列出所有网络服务信息;
networksetup -setv6off "Ethernet"  //停止对应网卡的ipV6服务;
networksetup -setdhcp  Ethernet  // 设置网卡的地址为DHCP；
networksetup -setmanual "Ethernet" 192.168.2.222 255.255.255.0 192.168.2.1   //手动设置网卡的ip地址;

networksetup -setmanualwithdhcprouter "Ethernet" 192.168.5.233
```



作者：Osworth
链接：https://www.jianshu.com/p/e07d2ecfa3b2
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

