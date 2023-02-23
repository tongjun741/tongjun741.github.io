---
tags:
    - 操作系统
    - Windows
---

nginx 安装、代理与gzip

1.从nginx官网下载相应的安装包

2.建议下载  下载稳定版

3.解压到相应的目录，比如是e盘 然后修改目录名字为nginx



Windows下Nginx的启动、停止等命令

在Windows下使用Nginx，我们需要掌握一些基本的操作命令，比如：启动、停止Nginx服务，重新载入Nginx等，下面我就进行一些简单的介绍。

1、启动：

C:\server\nginx-1.0.2>start nginx

或

C:\server\nginx-1.0.2>nginx.exe

注：建议使用第一种，第二种会使你的cmd窗口一直处于执行中，不能进行其他命令操作。

2、停止：

C:\server\nginx-1.0.2>nginx.exe -s stop

或

C:\server\nginx-1.0.2>nginx.exe -s quit



注：stop是快速停止nginx，可能并不保存相关信息；quit是完整有序的停止nginx，并保存相关信息。

3、重新载入Nginx：

C:\server\nginx-1.0.2>nginx.exe -s reload

当配置信息修改，需要重新载入这些配置时使用此命令。

4、重新打开日志文件：

C:\server\nginx-1.0.2>nginx.exe -s reopen

5、查看Nginx版本：

C:\server\nginx-1.0.2>nginx -v



开启带gzip的代理：

http{

server {
    listen 0.0.0.0:80;
    server_name blog.jackyang.me;

    location / {
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_set_header X-NginX-Proxy true;
      proxy_pass http://blog.jackyang.me/;
      proxy_redirect off;
    }
    
    # new config lines for gzip
    gzip on;
    gzip_min_length 1k;
    gzip_buffers 4 8k;
    gzip_http_version 1.1;
    gzip_types text/plain application/javascript application/x-javascript text/javascript text/css application/xml;
    
    location /static/ {
        root /var/www/jackyang.me/blog;
    }
 }

}





https://zhidao.baidu.com/question/1113304992354028179.html



http://blog.csdn.net/ppby2002/article/details/38681345



https://segmentfault.com/a/1190000004537991

