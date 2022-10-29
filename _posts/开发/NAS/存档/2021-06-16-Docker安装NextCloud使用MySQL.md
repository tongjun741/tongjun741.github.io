---
tags:
    - NAS
    - 存档
---

Docker安装NextCloud使用MySQL

安装

1.拉取并启动MySQL：

docker run --name=nextcloud_db -e MYSQL_ROOT_PASSWORD=12345678 -d -p 33306:3306 mysql:5

2.创建nextcloud数据库：

-  docker exec -it nextcloud_db mysql -u root -p
-  CREATE DATABASE nextcloud;
-  GRANT ALL ON *.* TO 'root'@'%';
-  flush privileges;
-  exit;


3.拉取并启动NextCloud：

docker run --name=nextcloud --link nextcloud_db:db -p 8888:80 -d nextcloud

4.浏览器访问宿主机IP:8888进行注册，填写MySQL主机：宿主机IP：33306

内网穿透

1. 注册账号Sakura账号，并配置映射信息

2.下载客户端Sakura至Docker宿主机：

wget https://cdn.tcotp.cn:4443/client/Sakura_frpc_linux_amd64.tar.gz

3.开启端口转发：

Sakura_frpc_linux_amd64

4.访问Sakura生成的地址访问并注册NextCloud账号

5.若出现信任域问题，编辑NextCloud配置文件：

vim /var/www/html/config/config.php

在trusted_domains处添加对应地址



https://www.cnblogs.com/steinven/p/11357295.html

