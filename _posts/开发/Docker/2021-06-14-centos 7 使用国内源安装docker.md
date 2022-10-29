---
tags:
    - Docker
---

centos 7 使用国内源安装docker

sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
sudo yum makecache fast
sudo yum -y install docker-ce

参考：https://www.cnblogs.com/whgfu/articles/9466859.html

最新docker engine: https://docs.docker.com/engine/release-notes/