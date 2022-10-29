---
tags:
    - 其它
    - Jira及Confluence
---

现象：

```
Tomcat 配置不正确
 
The Tomcat server.xml 配置不正确:
 
scheme should be 'https'
proxyName should be 'wiki.xxx.com'
proxyPort should be 'https'
```

找了很多网站都说要修改/opt/atlassian/confluence/conf/server.xml，但我这用Docker映射了server.xml文件，修改server.xml之后重启confluence就会被重置。



解决方案是使用环境变量定义scheme、proxyName和proxyPort：

```
  confluence:
    build: ./confluence
    container_name: app_confluence
    restart: always
    environment:
      - ATL_PROXY_NAME=wiki.xxx.com
      - ATL_PROXY_PORT=443
      - ATL_TOMCAT_SCHEME=https
```





https://blog.csdn.net/sinat_41542983/article/details/115319306

https://confluence.atlassian.com/jirakb/set-ssl-using-docker-container-1014274479.html