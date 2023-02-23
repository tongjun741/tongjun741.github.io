---
tags:
    - Docker
---

Docker pull php-fpm的php.ini配置修改



```javascript
今天，换了 Deepin 操作系统，开发环境是通过 Docker 搭建的，具体结构如下：
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
ef6fadf6ebf0        nginx               "nginx -g 'daemon of…"   19 hours ago        Up 2 hours          0.0.0.0:8080->80/tcp     nginx
edb6bc0ad5bc        php:7.1-fpm         "docker-php-entrypoi…"   19 hours ago        Up 2 hours          0.0.0.0:9000->9000/tcp   php7.1
c77df990721e        mysql:5.6           "docker-entrypoint.s…"   23 hours ago        Up 3 hours          0.0.0.0:3306->3306/tcp   mysql5.6
其中 php 的容器是通过下面的命令安装的：
docker pull php:7.1-fpm
现在在项目中需要上传文件，但是镜像提供的php的上传配置都是：
upload_max_filesize=2M
post_max_size=2M
明显不足，所以需要修改下配置文件，但是找了半天没有找到 php.ini 的配置文件在哪里，最终，可以这样操作：
cd /usr/local/etc/php/conf.d

vi upload.ini
输入下面的内容：
upload_max_filesize=100M
post_max_size=100M
这样，就配置完成了。为什么这样可以呢？因为 /usr/local/etc/php/conf.d 目录下面的 ini 配置文件会自动加载的。

```



https://www.cnblogs.com/agang-php/p/11506386.html

