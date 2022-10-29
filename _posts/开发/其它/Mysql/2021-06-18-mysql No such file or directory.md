---
tags:
    - 其它
    - Mysql
---

mysql No such file or directory

先在phpinfo.php中确定php.ini的位置

然后使用mysql_config --socket确认mysql.socket的位置

修改php.ini中的mysql.default_socket、mysqli.default_socket、PDO_mysql.default_socket

重启php-fpm:

sudo killall php-fpm

sudo php-fpm





http://stackoverflow.com/questions/748478/cannot-find-mysql-sock

