---
tags:
    - 其它
    - Jenkins
---

错误提示只有这么一点：

```
Failed to connect session for config [rocket.chat.thinkoncloud.cn]. Message [Auth fail]
```



具体信息需要在ssh服务端查看：

```
sudo tail -f /var/log/auth.log
```

一般可以看到：

```
Jan  8 11:51:21 iZmj7c8vjlin0mesh6v6fyZ sshd[37539]: error: Received disconnect from xx.xx.xx.xx port 41538:3: com.jcraft.jsch.JSchException: Auth fail [preauth]
```



## 解决方案：

```
sudo vim /etc/ssh/sshd_config

# 添加以下行：
PubkeyAcceptedKeyTypes +ssh-rsa

# 重启sshd服务：
sudo service sshd restart
```



https://stackoverflow.com/questions/65015826/jenkins-plugins-publish-over-bappublisherexception-failed-to-connect-and-initia