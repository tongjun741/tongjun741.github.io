---
tags:
    - 操作系统
    - Mac
---

macbook开机启动nginx和php

1. 开机启动nginx：

在 /Library/LaunchDaemons/ 目录新建 org.macports.nginx.plist 文件：

[html] view plain copy

1. <?xml version='1.0' encoding='UTF-8'?>  

1. <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"  

1. "http://www.apple.com/DTDs/PropertyList-1.0.dtd" >  

1. <plist version='1.0'>  

1. <dict>  

1. <key>Label</key><string>org.macports.nginx</string>  

1. <key>ProgramArguments</key>  

1. <array>  

1.     <string>/opt/local/bin/daemondo</string>  

1.     <string>--label=nginx</string>  

1.     <string>--start-cmd</string>  

1.     <string>/usr/bin/nginx</string>  

1.     <string>;</string>  

1.     <string>--pid=fileauto</string>  

1.     <string>--pidfile</string>  

1.     <string>/opt/local/var/run/nginx/nginx.pid</string>  

1. </array>  

1. <key>Debug</key><false/>  

1. <key>Disabled</key><true/>  

1. <key>KeepAlive</key><true/>  

1. </dict>  

1. </plist>  



加载配置：  launchctl load -w /Library/LaunchDaemons/org.macports.nginx.plist



2. 开机启动PHP：

在 /Library/LaunchDaemons/ 目录新建 org.php.php-fpm.plist 文件：

[html] view plain copy

1. <?xml version="1.0" encoding="UTF-8"?>  

1. <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">  

1. <plist version="1.0">  

1. <dict>  

1.     <key>Label</key>  

1.     <string>php-fpm</string>  

1.     <key>Program</key>  

1.     <string>/User/wyt/module/php/sbin/php-fpm</string>  

1.     <key>KeepAlive</key><true/>  

1. </dict>  

1. </plist>  



launchctl load -w /Library/LaunchDaemons/org.php.php-fpm.plist



http://blog.csdn.net/oceanwavewyt/article/details/38460331





安装homebrew

homebrew是mac下非常好用的包管理器，会自动安装相关的依赖包，将你从繁琐的软件依赖安装中解放出来。 安装homebrew也非常简单，只要在终端中输入:

<!-- lang: shell -->
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"


homebrew的常用命令:

<!-- lang: shell -->
brew update #更新可安装包的最新信息，建议每次安装前都运行下
brew search pkg_name #搜索相关的包信息
brew install pkg_name #安装包


想了解更多地信息，请参看homebrew

安装nginx

安装

<!-- lang: shell -->
brew search nginx
brew install nginx


当前的最新版本是1.4.4。

配置

<!-- lang: shell -->
cd /usr/local/etc/nginx/
mkdir conf.d
vim nginx.conf
vim ./conf.d/default.conf


nginx.conf内容,

<!-- lang: shell -->
worker_processes  1;  

error_log       /usr/local/var/log/nginx/error.log warn;

pid        /usr/local/var/run/nginx.pid;

events {
    worker_connections  256;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log      /usr/local/var/log/nginx/access.log main;
    port_in_redirect off;
    sendfile        on; 
    keepalive_timeout  65; 

    include /usr/local/etc/nginx/conf.d/*.conf;
}


default.conf文件内容,

<!-- lang: shell -->
server {
    listen       8080;
    server_name  localhost;

    root /Users/user_name/nginx_sites/; # 该项要修改为你准备存放相关网页的路径

    location / { 
        index index.php;
        autoindex on; 
    }   

    #proxy the php scripts to php-fpm  
    location ~ \.php$ {
        include /usr/local/etc/nginx/fastcgi.conf;
        fastcgi_intercept_errors on; 
        fastcgi_pass   127.0.0.1:9000; 
    }   

}


安装php-fpm

Mac OSX 10.9的系统自带了PHP、php-fpm，省去了安装php-fpm的麻烦。 这里需要简单地修改下php-fpm的配置，否则运行php-fpm会报错。

<!-- lang: shell -->
sudo cp /private/etc/php-fpm.conf.default /private/etc/php-fpm.conf
vim /private/etc/php-fpm.conf


修改php-fpm.conf文件中的error_log项，默认该项被注释掉，这里需要去注释并且修改为error_log = /usr/local/var/log/php-fpm.log。如果不修改该值，运行php-fpm的时候会提示log文件输出路径不存在的错误。

安装mysql

安装

<!-- lang: shell -->
brew install mysql


常用命令

<!-- lang: shell -->
mysql.server start #启动mysql服务
mysql.server stop #关闭mysql服务


配置 在终端运行mysql_secure_installation脚本，该脚本会一步步提示你设置一系列安全性相关的参数，包括：设置root密码，关闭匿名访问，不允许root用户远程访问，移除test数据库。当然运行该脚本前记得先启动mysql服务。

测试nginx服务

在之前nginx配置文件default.conf中设置的root项对应的文件夹下创建测试文件index.php:

<!-- lang: php -->
<!-- ~/nginx_sites/index.php -->
<?php phpinfo(); ?>


启动nginx服务，sudo nginx； 修改配置文件，重启nginx服务，sudo nginx -s reload 启动php服务，sudo php-fpm； 在浏览器地址栏中输入localhost:8080，如果配置正确地话，应该能看到PHP相关信息的页面。

https://my.oschina.net/chen0dgax/blog/190161





