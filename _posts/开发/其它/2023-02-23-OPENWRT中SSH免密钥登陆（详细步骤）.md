---
tags:
    - 其它
---

通过使用ssh-keygen生成公钥，在两台机器之间互相建立新人通道极客。



假设本地机器是client，远程机器为server。



1、使用ssh-keygen生成rsa keygen（在这里会覆盖以前生成的~/.ssh/id_rsa文件，请提前做好备份。）



ssh-keygen -b 1024 -t -rsa

然后一直按回车即可。



2、拷贝公钥到目标机器上，并更名为authorized_keys

scp ~/.ssh/id_rsa.pub root@192.168.8.1:/etc/dropbear/authorized_keys



3、拷贝完成后，正常登陆一次目标主机。

4、退出后重新登陆，这个时候就不在需要ssh密钥就可以登陆目标主机了。





https://blog.csdn.net/li6727975/article/details/44451717