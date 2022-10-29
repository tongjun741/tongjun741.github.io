---
tags:
    - Docker
---

docker 启动的时候增加参数--privileged ，开启特权，可以设置容器里的内核参数。

进入容器，修改容器文件 /etc/sysctl.conf

容器内执行sysctl -p 即可生效，但是如果重启容器，会失效，需要再次执行。



作者：testerzhang
链接：https://www.jianshu.com/p/da9a2f49880c
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。





对于已经创建好的容器可以停止后修改配置文件：

```
# docker run -ti --name test fedora:25 /bin/bash
# echo 512 > /proc/sys/net/core/somaxconn   # in docker
bash: /proc/sys/net/core/somaxconn: Read-only file system
# exit # exit docker, back to host
# systemctl stop docker # or stop it with whatever servicemanager you're using

# cd /var/lib/docker/containers/b48fcbce0ab29749160e5677e3e9fe07cc704b47e84f7978fa74584f6d9d3c40/
# cp hostconfig.json{,.bak}
# cat hostconfig.json.bak | jq '.Privileged=true' | jq '.SecurityOpt=["label=disable"]' > hostconfig.json

# systemctl start docker
# docker start test
test
# docker exec -ti test /bin/bash
# echo 512 > /proc/sys/net/core/somaxconn   # in docker, now works
```

https://serverfault.com/questions/861227/restart-docker-container-in-privileged-mode