---
tags:
    - 其它
    - Nginx
---

nginx php-fpm 输出php错误日志

nginx是一个web服务器，因此nginx的access日志只有对访问页面的记录，不会有php 的 error log信息。

nginx把对php的请求发给php-fpm fastcgi进程来处理，默认的php-fpm只会输出php-fpm的错误信息，在php-fpm的errors log里也看不到php的errorlog

原因是php-fpm的配置文件php-fpm.conf中默认是关闭worker进程的错误输出，直接把他们重定向到/dev/null,所以我们在nginx的error log 和php-fpm的errorlog都看不到php的错误日志。

调试起来就很痛苦了。解决nginx下php-fpm不记录php错误日志的办法:

1.修改php-fpm.conf中配置 没有则增加

catch_workers_output = yes

error_log = log/error_log

2.修改php.ini中配置，没有则增加

log_errors = On

error_log = "/usr/local/lnmp/php/var/log/error_log"

error_reporting=E_ALL&~E_NOTICE

3.重启php-fpm，

当PHP执行错误时就能看到错误日志在"/usr/local/lnmp/php/var/log/error_log"中了

请注意：

1. php-fpm.conf 中的php_admin_value[error_log] 参数 会覆盖php.ini中的 error_log 参数

所以确保你在phpinfo()中看到的最终error_log文件具有可写权限并且没有设置php_admin_value[error_log] 参数，否则错误日志会输出到php-fpm的错误日志里。

2.找不到php.ini位置，使用php的phpinfo()结果查看

3.如何修改PHP错误日志不输出到页面或屏幕上

修改php.ini

display_errors = off //不显示错误信息(不输出到页面或屏幕上)

log_errors = on //记录错误信息(保存到日志文件中)

error_reporting = E_ALL //捕获所有错误信息

error_log = //设置日志文件名

程序中修改以上配置

ini_set("display_errors",0)

ini_set("error_reporting",E_ALL); //这个值好像是个PHP的常量

ini_set("error_log","<日志文件名>")

ini_set("log_errors",1);

4.如何将php的错误日志输出到nginx的错误日志里

在PHP 5.3.8及之前的版本中，通过FastCGI运行的PHP，在用户访问时出现错误，会首先写入到PHP的errorlog中

如果PHP的errorlog无法写入，则会将错误内容返回给FastCGI接口，然后nginx在收到FastCGI的错误返回后记录到了nginx的errorlog中

在PHP 5.3.9及之后的版本中，出现错误后PHP只尝试写入PHP的errorlog中，如果失败则不会再返回到FastCGI了，错误日志会输出到php-fpm的错误日志里。

所以如果想把php错误日志输出到nginx错误日志，需要使用php5.3.8之前的版本，并且配置文件中php的error_log对于php worker进程不可写

生命只有一次。





http://www.cnblogs.com/glory-jzx/p/3966082.html

