---
tags:
    - Docker
---

起因是使用docker搭建了clickhouse测试环境，docker的启动命令如下：



```bash
$ docker run -d --name clickhouse-server --ulimit nofile=262144:262144 -p 8123:8123 -p 9000:9000 yandex/clickhouse-server
```

但是由于后期使用需要修改clickhouse的时区，必须使用自定义的配置文件。有以下几种实现方法，比较理想的是第三种：：

- 1. 删除原有容器，重新建新容器。这样最简单，但是会丢失数据。



```ruby
$ docker rm clickhouse-server
$ docker run -d --name clickhouse-server --ulimit nofile=262144:262144 -p 8123:8123 -p 9000:9000 -v /data/application/clickhouse_server:/etc/clickhouse-server/ yandex/clickhouse-server
```

- 1. 提交现有容器为新镜像，然后重新运行它



```shell
$ docker ps  -a
CONTAINER ID        IMAGE                      COMMAND                  CREATED             STATUS                       PORTS               NAMES
8a553c83b636        yandex/clickhouse-server   "/entrypoint.sh"         2 months ago        Exited (0) 27 minutes ago                        clickhouse-server
$ docker commit 8a553c83b636 my-private-clickhouse-server
$ docker run -d -v /data/application/clickhouse_server:/etc/clickhouse-server/  my-private-clickhouse-server
```

- 1. 修改配置文件（需停止docker服务）
      MAC OS中，docker有两层虚拟机，一层是docker本身的虚拟层（linux），然后是docker里容器的虚拟层，所以网上文章提到的/var/lib/docker 文件夹在MAC系统中是找不到的。下面记录如何在mac系统中修改配置文件。

#### 1. 确定容器id



```shell
$ docker container inspect clickhouse-server
```

或者



```shell
$ docker ps |grep clickhouse
```

#### 2. 停止容器



```shell
$ docker stop clickhouse-server
```

#### 3. 进入docker虚拟的linux



```shell
$ cd ~/Library/Containers/com.docker.docker/Data/vms/0/
```

目录中，有一个tty文件，可以通过这个文件登录到docker内部的linux：



```shell
$ screen tty
```

*注：Screen是一款由GNU计划开发的用于命令行终端切换的自由软件。用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换。GNU Screen可以看作是窗口管理器的命令行界面版本。它提供了统一的管理多个会话的界面和相应的功能。*

#### 4. 编辑config.v2.json和hostconfig.json

在虚拟linux系统中



```shell
$ vi /var/lib/docker/containers/{容器id}/config.v2.json
$ vi /var/lib/docker/containers/{容器id}/hostconfig.json
```

##### 在hostconfig.json中增加挂载：



```bash
"Binds": [
    "/data/application/clickhouse_server:/etc/clickhouse-server/"
  ],
```

##### 在config.v2.json中增加挂载：



```bash
"MountPoints": {
    "/etc/clickhouse-server": {
      "Source": "/data/application/clickhouse_server",
      "Destination": "/etc/clickhouse-server",
      "RW": true,
      "Name": "",
      "Driver": "",
      "Type": "bind",
      "Propagation": "rprivate",
      "Spec": {
        "Type": "bind",
        "Source": "/data/application/clickhouse_server",
        "Target": "/etc/clickhouse-server/"
      },
      "SkipMountpointCreation": false
    },
    "/var/lib/clickhouse": {
      "Source": "",
      "Destination": "/var/lib/clickhouse",
      "RW": true,
      "Name": "bf64389240fe2a56968bc8e7cb15706cfcff97890f35a072f92fa40da19e700b",
      "Driver": "local",
      "Type": "volume",
      "Spec": {},
      "SkipMountpointCreation": false
    }
  }
```

修改完成后，保存。

#### 5. 退出Linux（重要）

这一步很重要，因为没有退出，所以开始尝试很多次，配置文件都旧文件覆盖而没有生效。
 按Ctrl+A+D，退出screen命令。然后，使用screen -ls命令，查看当前会话：



```shell
$ screen -ls
There is a screen on:
    41557.ttys004.YandeMacBook-Pro  (Detached)
1 Socket in /var/folders/d0/y2ythxgd2p54y1p46bv90wpm0000gn/T/.screen.
```

##### 使用kill命令杀死会话：



```shell
$ kill -9 41557
```

##### 退出会话：



```shell
$ screen -wipe
There is a screen on:
    41557.ttys004.YandeMacBook-Pro  (Removed)
1 socket wiped out.
No Sockets found in /var/folders/d0/y2ythxgd2p54y1p46bv90wpm0000gn/T/.screen.
```

#### 6. 重启docker

必须重启，否则配置文件内容还是会被还原

#### 8. 准备配置文件

到clickhouse的github代码库中，下载配置文件config.xml和users.xml放到目录/data/application/clickhouse_server 中，修改配置文件增加时区，允许远程访问等。
 在config.xml中添加：



```xml
<timezone>Asia/Shanghai</timezone>
<listen_host>0.0.0.0</listen_host>
```

#### 9. 启动镜像



```shell
$ docker start clickhouse-server
```

查看配置已经生效



作者：EdgeE
链接：https://www.jianshu.com/p/17c0fc4c024f
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。