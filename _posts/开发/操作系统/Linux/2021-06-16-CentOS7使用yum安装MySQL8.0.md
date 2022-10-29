---
tags:
    - 操作系统
    - Linux
---

CentOS7使用yum安装MySQL8.0



```javascript
1、yum仓库下载MySQL：sudo yum localinstall https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm
2、yum安装MySQL：sudo yum install mysql-community-server
3、启动MySQL服务：sudo service mysqld start
4、检查MySQL服务状态：sudo service mysqld status
5、查看初始密码（如无内容直接跳过）：sudo grep 'temporary password' /var/log/mysqld.log
6、本地MySQL客户端登录：mysql -uroot -p
7、输入密码为第5步查出的，如果没有，直接回车，然后输入命令  flush privileges
8、修改root登录密码：ALTER USER 'root'@'localhost' IDENTIFIED BY '密码';（注意要切换到mysql数据库，使用use mysql）
注意：开始遇到问题是不输入密码或输错密码都能连接MySQL server，后来在修改允许阿里CentOS7允许远程操作MySQL数据库时，
才发现需要去调整 /etc/my.cnf文件，注释掉skip-grant-tables，重启MySQL服务（sudo service mysqld restart），quit退出连接，重新连接就需要输入密码了
后期如果忘记密码，可以通过skip-grant-tables配置跳过输入密码登录MySQL，执行7、8步进行修改，如果‘root’@'localhost'变为'root'@'%'，那么alter语句中的也要修改
9、配置MySQL允许外部访问：1）首先设置阿里云安全组规则入方向，支持MySQL端口3306可访问（协议类型下拉菜单中有MySQL端口）
　　　　　　　　　　　　　　2）服务端登录MySQL，use mysql;然后执行select user,host from user可查看用户及对应允许访问主机
　　　　　　　　　　　　　　　　然后执行update user set host = '%' where user ='root';允许任何外部可访问；再执行上一步查看命令，可比较结果
10、如此即可连接
补充：show global variables like 'port';可查看MySQL服务端口，如果看到的value为0，则说明没有使用密码登录，需要去修改my.cnf文件；
my.cnf文件也可以通过port=3306来指定MySQL服务端口，重启MySQL服务即可
11、java连接8.0及以上MySQL数据库使用新驱动
这个问题是在我用本地工具可以连同阿里云服务器上的MySQL，而本地用java怎么也连不上，偶然间点开工具测试连接的详细信息发现新的驱动，更改java对应驱动后，连接成功

```

https://www.cnblogs.com/hujiapeng/p/9124298.html



无法修改密码的问题是因为默认密码策略要求比较高，先改成一个复杂密码再把密码策略要求改低

```javascript
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> SHOW VARIABLES LIKE 'validate_password%';
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123@abc!!!A';
Query OK, 0 rows affected (0.01 sec)

mysql> SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.01 sec)
mysql> set global validate_password.policy=LOW;
Query OK, 0 rows affected (0.00 sec)
mysql> set global  validate_password.length=1;
Query OK, 0 rows affected (0.00 sec)

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
Query OK, 0 rows affected (0.00 sec)


```

https://blog.csdn.net/hello_world_qwp/article/details/79551789

