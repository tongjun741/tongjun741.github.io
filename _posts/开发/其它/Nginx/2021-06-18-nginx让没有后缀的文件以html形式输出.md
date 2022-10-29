---
tags:
    - 其它
    - Nginx
---

nginx让没有后缀的文件以html形式输出

location ~ something {
   default_type text/html;
}



http://stackoverflow.com/questions/19629930/force-nginx-to-send-specific-content-type

