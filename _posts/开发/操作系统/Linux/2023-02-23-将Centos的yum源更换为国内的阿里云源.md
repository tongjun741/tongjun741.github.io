---
tags:
    - 操作系统
    - Linux
---

将Centos的yum源更换为国内的阿里云源

阿里云Linux安装软件镜像源

阿里云是最近新出的一个镜像源。得益与阿里云的高速发展，这么大的需求，肯定会推出自己的镜像源。

阿里云Linux安装镜像源地址：http://mirrors.aliyun.com/

CentOS系统更换软件安装源

第一步：备份你的原镜像文件，以免出错后可以恢复。

mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

第二步：下载新的CentOS-Base.repo 到/etc/yum.repos.d/

CentOS 5

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo

CentOS 6

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo

第三步：运行yum makecache生成缓存

yum clean all

yum makecache



http://www.centoscn.com/CentOS/config/2015/0416/5189.html

