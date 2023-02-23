---
tags:
    - 其它
    - Jira及Confluence
---

修改./confluence/Dockerfile增加一行：

```
RUN echo 'export CATALINA_OPTS="-Datlassian.recovery.password=123456 ${CATALINA_OPTS}"' >> /opt/atlassian/confluence/bin/setenv.sh
```



重启后使用recovery_admin/123456登录：

```
docker-compose up -d --build
```





https://www.jianshu.com/p/36f0bd7a974f