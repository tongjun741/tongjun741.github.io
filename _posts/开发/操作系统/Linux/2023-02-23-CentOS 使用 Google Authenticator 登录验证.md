---
tags:
    - 操作系统
    - Linux
---

CentOS 使用 Google Authenticator 登录验证

手机上安装Google身份验证器

安装地址：http://www.wandoujia.com/apps/com.google.android.apps.authenticator2



注意：机器上一定要关闭selinux



1、安装依赖：

yum -y install gcc make pam-devel libpng-devel libtool wget git



2、开启EPEL源 

yum –enablerepo=epel



3、或者直接安装EPEL源RPM包

# CentOS 6

rpm -Uvh http://mirrors.ustc.edu.cn/fedora/epel/epel-release-latest-6.noarch.rpm

# CentOS 7

rpm -Uvh http://mirrors.ustc.edu.cn/fedora/epel/epel-release-latest-7.noarch.rpm



4、安装Qrencode，谷歌身份验证器需要调用该程序生成二维码并显示

yum install -y qrencode



5、安装谷歌身份验证器

git clone https://github.com/google/google-authenticator-libpam.git
cd google-authenticator-libpam/



编译并安装

./bootstrap.sh
./configure --prefix=/usr/local/google-authenticator
make && make install



复制google 身份验证器pam模块到系统下

cp /usr/local/google-authenticator/lib/security/pam_google_authenticator.so /lib64/security/



6、配置/etc/pam.d/sshd

在

auth       include      password-auth

这一行上面添加下面这行内容

auth       required     pam_google_authenticator.so

注意顺序：谷歌认证要在password-auth上面



[root@localhost ~]# cat /etc/pam.d/sshd 

#%PAM-1.0
auth       required     pam_sepermit.so
auth       required     pam_google_authenticator.so 
auth       include      password-auth
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    optional     pam_keyinit.so force revoke
session    include      password-auth



7、修改SSH服务配置/etc/ssh/sshd_config 

将ChallengeResponseAuthentication no改成yes，即

ChallengeResponseAuthentication yes



8、启用 Google Authenticator

./google-authenticator

Do you want authentication tokens to be time-based (y/n) y

# 是否开启基于时间的认证，Y， 测试下来，如果选N，则手机上的验证码不会自动更新，使用一次后就算手动更新了验证码也无法登录。

# 接下来会生成二维码，手机端扫描即可添加安全密钥

后面一路都是选择y，就可以了

注意保存好上面的5个emergency scratch codes，如果手机上的验证码不通过，可以使用上面的这个验证码，每次使用后就失效了。





linux登录客户端的设置



参考文档：

https://shenyu.me/2016/09/05/centos-google-authenticator.html

https://www.sulabs.net/?p=802



