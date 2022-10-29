---
tags:
    - 其它
    - Nginx
---

nginx根据当前浏览器ua判断是否是移动版决定是否跳转到pc版

 server {

        listen       1884;

        server_name  aa.aaa.com;



        #charset koi8-r;



        #access_log  logs/host.access.log  main;



        location / {

            root   /Users/user/workspace/www;

            index  default.html index.html index.htm index.php;





                #用于PC和mobile的自动跳转

                location ~ ^\/((default|price|videos|contact|pages\/.*|cj\/default)\.html|cj\/)?$ {

                        if ($http_user_agent ~* (mobile|IEMobile|MIDP|SymbianOS|NOKIA|SAMSUNG|LG|NEC|TCL|Alcatel|BIRD|DBTEL|Dopod|PHILIPS|HAIER|LENOVO|MOT-|Nokia|SonyEricsson|SIE-|Amoi|ZTE)) {

                                rewrite  ^(.*)    /mobile$1 permanent;

                        }

                }



                location ~ ^\/mobile\/((default|price|videos|contact|pages\/.*|cj\/default)\.html|cj\/)?$ {

                        if ($http_user_agent !~* (mobile|IEMobile|MIDP|SymbianOS|NOKIA|SAMSUNG|LG|NEC|TCL|Alcatel|BIRD|DBTEL|Dopod|PHILIPS|HAIER|LENOVO|MOT-|Nokia|SonyEricsson|SIE-|Amoi|ZTE)) {

                                rewrite  ^\/mobile\/(.*)    /$1 permanent;

                        }

                }

        }



        #...



}

