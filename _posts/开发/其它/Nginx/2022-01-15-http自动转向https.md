---
tags:
    - 其它
    - Nginx
---

```
 server {
        listen 80;
        server_name 你的域名;
        rewrite ^(.*)$ https://$host$1 permanent;
}
```

https://www.cnblogs.com/gxp69/p/11927405.html