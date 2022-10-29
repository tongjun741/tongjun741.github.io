---
tags:
    - 其它
    - Nginx
---

nginx php-fpm Primary script unknown

FastCGI sent in stderr: “Primary script unknown” while reading response header from upstream



在centos上成功编译安装nginx 1.4、php 5.4并成功启动nginx和php-fpm后，访问php提示"File not found."，同时在错误日志中看到：

复制代码代码如下:

2013/10/22 20:05:49 [error] 12691#0: *6 FastCGI sent in stderr: "Primary script unknown" while reading response header from upstream, client: 192.168.168.1, server: localhost, request: "GET / HTTP/1.1", upstream: "fastcgi://127.0.0.1:9000", host: "192.168.168.133"

错误解决方法：

在Nginx配置文件中找到定义调用脚本文件的地方，如：

复制代码代码如下:

fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;



修改成如下方式（$document_root）：

复制代码代码如下:

fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

提示：

$document_root 代表当前请求在root指令中指定的值。如：

复制代码代码如下:

location / {

       root   /usr/local/nginx/html;

       index  index.php index.html index.htm;

       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

}

上面配置中的$document_root就是针对/usr/local/nginx/html目录下的php文件进行解析。



http://www.jb51.net/article/47916.htm

