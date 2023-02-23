---
tags:
    - 其它
    - Mysql
---

完美解决Mysql的Access denied for user 'root'@'%'问题

   最近在分配mysql权限时出错,mysql版本5.6,造成mysql在重新分配权限提示"Access denied for user 'root'@'%"，出错原因reload权限被收回，造成无法重新分配权限，其他类似权限问题也可以参照此方法。



    一·解决办法 



    第一步：停服务

命令行：

/etc/init.d/mysql stop

如果不行，就执行下一行：

service mysqld stop

报：

Stopping mysqld:  [  OK  ]





第二步：跳过密码验证

执行命令行：

# /usr/bin/mysqld_safe --skip-grant-tables

报：

151104 09:07:56 mysqld_safe Logging to '/var/lib/mysql/iZ23dq2wm0jZ.err'.

151104 09:07:56 mysqld_safe Starting mysqld daemon with databases from /var/lib/mysql





第三步：无密码登录

执行命令行：

mysql -u root 





第四步：授权

mysql>

grant all privileges on *.* to 'root'@'localhost' identified by 'root' with grant option;

关键词解释：

root'@'localhost:是用户

root：是密码





问题一：发现无密码条件下，没有授权的写权限

The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement

解决方法：

mysql> set global read_only=0;//（关掉新主库的只读属性）

mysql>flush privileges;

grant all privileges on *.* to 'root'@'localhost' identified by 'root' with grant option;#再次重新授权

mysql>set global read_only=1;//(读写属性)

mysql>flush privileges;

mysql>exit;

（注意刷新是必须项）





第五步：重启数据库

service mysqld stop

报：

Stopping mysqld:  [  OK  ]





service mysqld start

报：

Starting mysqld:  [  OK  ]

或者

service mysqld restart

————————————————

版权声明：本文为CSDN博主「nicajonh」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/nicajonh/article/details/52311402

