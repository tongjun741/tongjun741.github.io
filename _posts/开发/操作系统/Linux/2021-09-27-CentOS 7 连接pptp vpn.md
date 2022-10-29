---
tags:
    - 操作系统
    - Linux
---

CentOS 7 连接pptp vpn



```javascript
yum install -y ppp pptp pptp-setup net-tools psmisc  -y

service firewalld stop
service iptables stop

pptpsetup --create vpn1 --server <IP of VPN> --username <VPN account>--password <VPN password> --encrypt --start

```

启动正常如下：

Using interface ppp0
Connect: ppp0 <--> /dev/pts/1
CHAP authentication succeeded
MPPE 128-bit stateless compression enabled
local  IP address 10.1.3.128
remote IP address 10.1.3.1


3 查看网络

ip a查看网络配置，如果看到ppp0虚拟网卡的配置，就是启动正常，否则，检查VPN连接，可以查看/etc/log/messages文件中的日志信息

4 添加路由

```javascript
route add -net 0.0.0.0 dev ppp0

```



参考地址：http://cloudnil.com/2015/09/22/centos7-vpn/#4-%E6%B7%BB%E5%8A%A0%E8%B7%AF%E7%94%B1











