---
tags:
    - 其它
    - Mysql
---

mysqldump Out of resources when opening file '._xxx.MYD' (Errcode_ 24) 解决

mysqldump: Got error: 23: Out of resources when opening file './xxx/xxx1.MYD' (Errcode: 24) when using LOCK TABLES



出现Out of resources when opening file './xxx.MYD' (Errcode: 24)错误是因为打开的文件数超过了my.cnf的--open-files-limit。open-files-limit选项无法在mysql命令行直接修改，必须在my.cnf中设定，最大值是65536。

 

重新启动/etc/init.d/mysql restart以后，发现

mysql> show global variables like 'open%';

+------------------+-------+

| Variable_name    | Value |

+------------------+-------+

| open_files_limit | 1024  | 

+------------------+-------+

1 row in set (0.00 sec)

并没有改变。赶快查看服务器的打开文件数设定的值（用ulimit -n查看），结果发现果然是1024。在修改服务器设置后，也改成了65536，重启服务还是没有改变。需要重新登录服务器再重启数据库服务就OK了。原来这个值会取数据库和服务器设定的最小值。

mysql> show global variables like 'open%';

+------------------+-------+

| Variable_name    | Value |

+------------------+-------+

| open_files_limit | 65536 |

+------------------+-------+

1 row in set (0.00 sec)



http://blog.sina.com.cn/s/blog_5fbb091801010yqt.html



