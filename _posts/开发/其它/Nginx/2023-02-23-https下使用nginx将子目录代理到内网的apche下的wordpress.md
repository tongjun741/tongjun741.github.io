---
tags:
    - 其它
    - Nginx
---

https下使用nginx将子目录代理到内网的apche下的wordpress

要实现的访问地址：https://www.xxx.com/club



先在apache下开一个VirtualHost,端口使用8113,目录指向/srv/www/htdocs/club,wordpress的文件放在/srv/www/htdocs/club/club/下（注意是两个club）,index.php的全路径是：/src/www/htdocs/club/club/index.php



在nginx下增加一个server（注意路径）:

location /club/ {

            proxy_pass http://127.0.0.1:8113/club/;

            proxy_set_header Host $host;

            proxy_set_header X-Real-IP $remote_addr;

            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        }



wordpress安装后，修改wp-includes/load.php，在is_ssl函数中的第一行前增加 return ture;



mysql下修改wordpress的配置：

update wp_options set option_value="https://www.xxx.com/club/" where option_name="siteurl";

update wp_options set option_value="https://www.xxx.com/club/" where option_name="home";





http://wordpress.stackexchange.com/questions/191747/how-can-i-have-nginx-serve-wordpress-at-blog

http://www.2zzt.com/jcandcj/5883.html

