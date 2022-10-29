---
tags:
    - 其它
---

# 网络情况

```
VPC(192.168.8.*)  ====  公司内网(转发服务器192.168.6.81以及其它192.168.6.*)
```

无需在出口路由器上做配置

# 1、购买阿里云VPN网关

注意一定要绑定交换机，否则会随机绑定一个交换机，导致此交换机无法删除。

另外需要确保VPC下的交换机网段与公司内网的网段不能有冲突，否则无法打通这两个网段。

如果购买的配置不正确可以通过自动退费https://usercenter2.aliyun.com/refund/refund退掉后重新购买。

# 2、创建用户网关

要点是输入正确的公司出口IP。

如果出口IP经常变的话需要写脚本调阿里云API重新创建：http://www.manzu.tech/2019/01/17/%E5%A6%82%E4%BD%95%E8%A7%A3%E5%86%B3%E5%8A%A8%E6%80%81IP%E5%AF%BC%E8%87%B4%E7%9A%84VPN%E5%A4%B1%E6%95%88/

# 3、创建IPsec连接

一定要设置路由模式为感兴趣流模式，本端网段需要输入VPC的网段，对端网段需要输入公司的内网网段。

创建完成后需要回到VPN网关的详情页面的“目的路由表”中添加路由条目，目标网段需要输入公司的内网网段，下一跳选择IPsec连接以及刚才创建的IPsec连接实例。

点击IPsec连接的下载对端配置记录以下信息:

```
VPN网关的IP：Remote
密钥：IkeConfig.Psk
```



# 4、公司内网转发服务器配置

准备一台CentOS 7.9 按https://help.aliyun.com/document_detail/65374.html?spm=a2c4g.11186623.6.603.492c7501M8XBl1操作

```
yum -y install epel-release
yum install strongswan  -y

```

修改/etc/strongswan/ipsec.conf  配置为以下内容：

```
config setup
     uniqueids=never
conn %default
     authby=psk
     type=tunnel
conn tomyidc
     keyexchange=ikev1
     left=192.168.6.81（一定要使用本机的内网IP）
     leftsubnet=192.168.6.0/24（公司的内网网段）
     leftid=192.168.6.81（一定要使用本机的内网IP）
     right=121.89.201.209（VPN网关的IP，通过阿里云控制台IPsec连接的下载对端配置查看Remote属性）
     rightsubnet=192.168.8.0/24（VPC的网段）
     rightid=121.89.201.209（VPN网关的IP）
     auto=route
     ike=aes-sha1-modp1024
     ikelifetime=86400s
     esp=aes-sha1-modp1024
     lifetime=86400s
     type=tunnel
```

示例：

![image-20210304205802064](/img-post/开发/其它/通过阿里云VPN网关允许VPC内ECS访问公司局域网的所有主机.assets/image-20210304205802064.png)

修改/etc/strongswan/ipsec.secrets 为以下内容：

```
192.168.6.81（本机的内网IP） 121.89.201.209（VPN网关的IP） : PSK xxx（密钥，通过阿里云控制台IPsec连接的下载对端配置查看Psk属性）
```

示例：

![image-20210304205918863](/img-post/开发/其它/通过阿里云VPN网关允许VPC内ECS访问公司局域网的所有主机.assets/image-20210304205918863.png)

打开系统转发配置：

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

执行以下命令启动strongSwan服务：

```
systemctl enable strongswan
systemctl start strongswan
```

设置防火墙允许VPC的ESC连接公司内网中的其它机器：

```
firewall-cmd --permanent --add-service=ipsec
firewall-cmd --permanent --add-masquerade
firewall-cmd --reload
```

# 

参考文档：

https://help.aliyun.com/document_detail/65279.html?spm=a2c4g.11186623.6.551.7c7e4281jRfxS3

https://help.aliyun.com/document_detail/65374.html?spm=a2c4g.11186623.6.603.492c7501M8XBl1

https://ask.csdn.net/questions/648730

https://dolphincn.github.io/strongswan/%E4%BD%BF%E7%94%A8strongswan%20%E6%90%AD%E5%BB%BA%E7%AB%99%E7%82%B9%E5%88%B0%E7%AB%99%E7%82%B9tunnel/

https://blog.csdn.net/puppylpg/article/details/64918562

https://cloud.tencent.com/developer/article/1505715