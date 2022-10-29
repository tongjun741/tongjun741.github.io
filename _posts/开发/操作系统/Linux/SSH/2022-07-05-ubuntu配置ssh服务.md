---
tags:
    - 操作系统
    - Linux
    - SSH
---

sudo apt-get update


1、安装SSH服务器

sudo apt-get install openssh-server
2、查看ssh服务是否启动

sudo ps -e |grep ssh
若没启动，则手动启动

sudo service ssh start
3、使用gedit修改配置文件”/etc/ssh/sshd_config”

sudo gedit /etc/ssh/sshd_config
把配置文件中的”PermitRootLogin without-password”加一个”#”号,把它注释掉；再增加一句”PermitRootLogin yes”；保存，修改成功

https://www.csdn.net/tags/MtTaMgysNTUwNzYtYmxvZwO0O0OO0O0O.html