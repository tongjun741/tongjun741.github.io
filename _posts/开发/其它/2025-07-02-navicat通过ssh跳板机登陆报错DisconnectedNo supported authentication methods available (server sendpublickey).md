---
tags:
    - 其它
---

1，在跳板机服务器上执行命令，检测日志

```
service sshd restart
```



2，再在navicat连接测试下，这时候看到错误日志输出错误：

![img](/img-post/开发/其它/navicat通过ssh跳板机登陆报错DisconnectedNo supported authentication methods available (server sendpublickey).assets/1667462318-image-1024x75.png)

3，这个错误是新版本openssh的特性，rsa类型密钥不在当前公钥识别类型中，导致无法连接。执行命令，然后重启sshd

```
echo "PubkeyAcceptedKeyTypes=+ssh-rsa" >>/etc/ssh/sshd_config 

service sshd restart
```



可能是我最近换成Ubuntu 22.04 64位导致的，默认带的openssh比较新的原因。



https://zwjun.cn/archives/494