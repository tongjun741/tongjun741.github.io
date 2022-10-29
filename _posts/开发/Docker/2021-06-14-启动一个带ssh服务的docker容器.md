---
tags:
    - Docker
---

```
# 准备一个Cent OS 7 的系统

# 使用root用户ssh进去后安装必要软件：
yum install wget curl telnet net-tools zip unzip ntpdate lsof vim bind-utils traceroute -y

#  安装docker服务
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
sudo systemctl start docker

# 设置代理
export http_proxy=http://192.168.5.233:8118
export https_proxy=http://192.168.5.233:8118

# 启动容器
docker run -it --rm  -d --name sshd15  -p 22215:22   -e "USERNAME=mylogin"   -e "PUBKEY=ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAvjE2JkJPq0yYUfa9HJXtP4SBG9trJ7sD2e91AMZjsddddd== root@mail"  -e "PASSWORD=123456" -e "SUDOER=nopasswd"   mdns/sshd

# 测试登录
ssh -i ~/aaa.key -o "StrictHostKeyChecking no" -p 22215 mylogin@127.0.0.1 "sudo  apt-get update;sudo apt-get install curl -y ; curl qq.com"

```

