---
tags:
    - 操作系统
    - Linux
---

CentOS 7 安装Mysql 5



```javascript
yum install mariadb-server mariadb -y

systemctl start mariadb
systemctl enable mariadb

mysql

MariaDB [(none)]> set password for 'root'@'localhost' =password('123456');



```

https://www.cnblogs.com/starof/p/4680083.html

