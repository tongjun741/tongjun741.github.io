---
tags:
    - NAS
---



#### 指定容器更新

如无需自动更新所有稳定运行的容器,可以配置仅更新指定容器,只需要在命令后加上容器名.例如只更新nginx和redis.

```
docker run -d \
    --name watchtower \
    --restart always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    containrrr/watchtower \
    --cleanup \
    nginx redis
```



如果使用文中的命令设置计划任务后运行一次，下次就没反应了，可以改为用此命令（创建并运行容器，并且在更新完成后删除自己）： docker run --rm --name watchtower -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --cleanup --run-once



前几篇文发了后有朋友在说什么时候写一下容器的自动更新，其实很简单的，今天来简单说下。下载containrrr/watchtower这个镜像，然后设置个定时任务即可。

首先打开[群晖](https://pinpai.smzdm.com/2315/)Docker搜索watchtower，并下载：

[![群晖使用Watchtower自动更新 Docker 映像与容器](/img-post/开发/NAS/群晖使用Watchtower自动更新 Docker 映像与容器.assets/6125daa30fd114127.jpg_e1080.jpg)](https://post.smzdm.com/p/akx8m8oe/pic_2/)

下载完成后不必配置和运行。

打开群晖控制面板，找到计划任务：

[![群晖使用Watchtower自动更新 Docker 映像与容器](/img-post/开发/NAS/群晖使用Watchtower自动更新 Docker 映像与容器.assets/6125db0b89969726.jpg_e1080.jpg)](https://post.smzdm.com/p/akx8m8oe/pic_3/)

点击计划任务，新增->计划的任务->用户定义的脚本：

[![群晖使用Watchtower自动更新 Docker 映像与容器](/img-post/开发/NAS/群晖使用Watchtower自动更新 Docker 映像与容器.assets/6125db544fa7d136.jpg_e1080.jpg)](https://post.smzdm.com/p/akx8m8oe/pic_4/)

常规里面任务名字自己顺便取，用户账号就用默认的root：

[![群晖使用Watchtower自动更新 Docker 映像与容器](/img-post/开发/NAS/群晖使用Watchtower自动更新 Docker 映像与容器.assets/6125db968deca250.jpg_e1080.jpg)](https://post.smzdm.com/p/akx8m8oe/pic_5/)

计划中，按自己需要设置，我设置的是每天6:00运行：

[![群晖使用Watchtower自动更新 Docker 映像与容器](/img-post/开发/NAS/群晖使用Watchtower自动更新 Docker 映像与容器.assets/6125dbcf1646f6718.jpg_e1080.jpg)](https://post.smzdm.com/p/akx8m8oe/pic_6/)

任务设置的运行命令中填入：

> docker run -d --name watchtower -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --cleanup --run-once

[![群晖使用Watchtower自动更新 Docker 映像与容器](/img-post/开发/NAS/群晖使用Watchtower自动更新 Docker 映像与容器.assets/6125dc842391e1749.jpg_e1080.jpg)](https://post.smzdm.com/p/akx8m8oe/pic_7/)

然后确定即可。

可以手动运行一次看看效果，如果有需要更新的容器watchtower会停止掉它，更新后再启动起来，系统通知也会收到容器异常停止的通知。

就这么简单，完事儿![群晖使用Watchtower自动更新 Docker 映像与容器](/img-post/开发/NAS/群晖使用Watchtower自动更新 Docker 映像与容器.assets/189.gif)



https://post.smzdm.com/p/akx8m8oe/

https://bytenote.net/article/125148794026196993