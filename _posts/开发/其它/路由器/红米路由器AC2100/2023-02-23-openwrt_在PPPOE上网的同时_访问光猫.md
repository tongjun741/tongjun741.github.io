---
tags:
    - 其它
    - 路由器
    - 红米路由器AC2100
---

https://www.cnblogs.com/osnosn/p/11857721.html



参考文章:

- [光猫桥接模式下,通过路由器访问光猫.简单设置](https://www.right.com.cn/forum/thread-400002-1-1.html)

## 设置OpenWRT

我用的是openwrt18.06版。
所有设置只需要通过web gui界面。不需要打命令。
光猫需要有dhcpd(通常无需修改光猫配置，光猫的缺省设置就是有dhcpd的)。

#### 首先，设置 openwrt 的 LAN 口 ipv4，为192.168.xx.1 (xx != 01)

openwrt的内网ipv4地址的网段,与光猫的ip段,**必须不同**。
通常光猫的ip是 192.168.1.1 (或192.168.0.1)
你可以设置 openwrt 为 192.168.10.1 或者 192.168.35.1 ...
改了 LAN IP 后，重新登陆进来。

#### 然后，创建一个新接口

[![img](/img-post/开发/其它/路由器/红米路由器AC2100/openwrt_在PPPOE上网的同时_访问光猫.assets/1641467-20191114153225663-289682603.png)](https://img2018.cnblogs.com/blog/1641467/201911/1641467-20191114153225663-289682603.png)

#### 取个名"modem"，选取dhcp client(DHCP客户端)，选择和WAN相同的设备，通常是eth0.2

[![img](/img-post/开发/其它/路由器/红米路由器AC2100/openwrt_在PPPOE上网的同时_访问光猫.assets/1641467-20191114153242355-668197505.png)](https://img2018.cnblogs.com/blog/1641467/201911/1641467-20191114153242355-668197505.png)

#### 去掉"缺省路由"的选项，去掉"DNS公告"

通常光猫不会提供DNS的。还是要,防止光猫抽筋,给你个DNS不能用。
[![img](/img-post/开发/其它/路由器/红米路由器AC2100/openwrt_在PPPOE上网的同时_访问光猫.assets/1641467-20191114153255706-1253750950.png)](https://img2018.cnblogs.com/blog/1641467/201911/1641467-20191114153255706-1253750950.png)

#### 防火墙区域，我选了unspecified。反正没什么规则可写。

如果无法访问光猫，试试把**防火墙区域设置为"WAN"**。

[![img](/img-post/开发/其它/路由器/红米路由器AC2100/openwrt_在PPPOE上网的同时_访问光猫.assets/1641467-20191114153306345-1865991176.png)](https://img2018.cnblogs.com/blog/1641467/201911/1641467-20191114153306345-1865991176.png)

#### 这就完成了。Save & Apply 提交一下。

如果不放心的话，重启一下openwrt。
你应该可以用 192.168.1.1 访问光猫了。
**并且,不影响正常上网，**
**不影响你的ipv4的端口映射，**
**不影响你的ipv6的forward规则。**

---end---
**转载注明来源: [本文链接](https://www.cnblogs.com/osnosn/p/11857721.html) 来自[osnosn的博客](https://www.cnblogs.com/osnosn/)**.