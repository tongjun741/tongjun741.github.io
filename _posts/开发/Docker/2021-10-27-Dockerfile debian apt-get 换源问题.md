---
tags:
    - Docker
---

```
使用以下命令替换源

RUN sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list
RUN cat /etc/apt/sources.list
RUN rm -Rf /var/lib/apt/lists/*
RUN apt-get update
其中 cat 命令 浏览文件确认源文件更替换完成

实际使用
关于镜像大小问题 减少 RUN 命令触发次数

RUN sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list && rm -Rf /var/lib/apt/lists/* && apt-get update

修改后建议重新 build
docker-compose build --no-cache workspace


————————————————
原文作者：莫须有
转自链接：https://learnku.com/articles/26066
版权声明：著作权归作者所有。商业转载请联系作者获得授权，非商业转载请保留以上作者信息和原文链接。
```

