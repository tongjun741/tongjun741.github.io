---
tags:
    - Docker
---

[回到顶部(Back to Top)](https://www.cnblogs.com/johnnyzen/p/18034076#_labelTop)

# 1 升级过程

## Step1 卸载低版本docker

### Step1.1 检查docker版本

```shell
# 查看版本(方法1)
docker version
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226120530903-163341283.png)

```shell
# 查看版本(方法2)
rpm -qa | grep docker
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226120601384-1479158596.png)

```shell
# 查看版本(方法3)
yum list installed | grep docker
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226120624690-220440459.png)

### Step1.3 查看已安装的镜像

```shell
# docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
superset-websocket            latest              71e3d9706e06        4 months ago        144MB
apache/superset               latest-dev          66d717d4c1bc        4 months ago        1.7GB
redis                         7                   e579380d4317        4 months ago        138MB
node                          16                  1ddc7e4055fd        5 months ago        909MB
demo_projects/py_helloworld   v1.0.0              77954c5df6a6        6 months ago        1GB
python                        3.9-alpine          6946662f018b        6 months ago        47.8MB
python                        3.9.18              5850a789011f        6 months ago        997MB
moby/buildkit                 buildx-stable-1     9291fad3b41c        6 months ago        172MB
postgres                      14                  251b1e989f6e        6 months ago        408MB
b3log/solo                    latest              c59c7acda4c3        9 months ago        369MB
mysql                         latest              05db07cd74c0        9 months ago        565MB
nginx                         1.22.0              08a1cbf9c69e        16 months ago       142MB


# 查看存放docker镜像的根路径
# ls /var/lib/docker/
builder  buildkit  containerd  containers  image  network  overlay2  plugins  runtimes  swarm  tmp  trust  volumes

# 查看存放docker镜像的镜像摘要信息
# ls /var/lib/docker/image/overlay2/imagedb/content/sha256/
05db07cd74c0520c8ffe5f7638063719a886f9115cecacc0654d981caf5d27f7  66d717d4c1bcdd9668428fbeb74c079a5e6c4e85d25bb7d62499164d59d46913
08a1cbf9c69edd2ab8e5250ae97703f60b9393fc5a4827cedda4b7387a5cfc6a  6946662f018b3519fa0b502df5d2af9479b239ea2b36d4db36c8ed848f006258
1ddc7e4055fdb6f6bf31063b593befda814294f9f904b6ddfc21ab1513bafa8e  71e3d9706e06c6d58a5e203afe8b9f029394be2e708f5f616770a18195c5f5d6
23e5d947b89f5ddf8ea2119bb04944386bbf52cb35451ffdd3f2baf758fdbcb8  77954c5df6a67d7886b59378e50018cae44ad024a9a5f4b9b664ce832fd1e1c9
251b1e989f6e62cd520b1aa29664600d3bd15f6d8808b00e0d679dee47f5984c  8ec8dde49a393abd31e8cf64340be595c3b934a25fb7830eadc9b3093ec1ad8d
40e68a1e9d7629272d609679819a2e200c28a562895baa98e9e719845927313c  9291fad3b41ccef145cd1f4ae73896687f19aaff54180d91b23911e5e6dffc8a
419d44c7e885788d940103164d77ec5ea90c691b7f5527f6fd29bd8c156a8800  c59c7acda4c39011ad076a5e852d2e1c563b450d60e35b101a8d1ddceed0fc1e
5850a789011f52b41cbb178ff92879fa352e3786f6ccfd4c78edaaaab7a902d9  e579380d43178bcee8c8b219063605f45e035238335db5d3b3e95c5e38145700

# 要查看Docker镜像存储的位置，也可使用docker inspect命令，该命令会输出Docker镜像的详细信息，包括存储位置。
docker inspect {imageId}
```

> Docker**镜像**的**默认存储路径**取决于操作系统的类型。
>
> - 对于Linux系统，默认存储路径是`/var/lib/docker`。这个目录下包含了多个子目录，如`image`、`containers`、`network`等，分别用于存储镜像、容器和网络数据。
> - 对于Windows系统，Docker的默认存储路径是`c:\programdata\dockerdesktop`。
> - 而对于macOS系统，默认的存储路径是`com.docker.docker/data/vms/0/`。
>   此外，用户可以通过修改Docker的配置文件`/etc/docker/daemon.json`来改变默认的存储路径。例如，可以将Docker的存储路径指向一个外部存储设备，如`/mnt/docker`。

### Step1.2 删除 docker

```shell
# 删除
# yum remove docker docker-common docker-client
或 yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
或 : yum -y remove docker* [√]

# 再次查看版本
# docker version
-bash: /usr/bin/docker: 没有那个文件或目录
```

> 注：不删除 `/var/lib/docker` 目录 就不会删除已安装的镜像及容器

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226121528158-1692145758.png)

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226121538008-319715488.png)

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226121617358-131701686.png)

## Step2 重新开始安装

### Step2.1 安装所需依赖

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226121920865-1058353425.png)

### Step2.2 设置 yum 源

```shell
yum-config-manager --add-repo http://download.docker.com/linux/centos/docker-ce.repo
  # 中央仓库
或
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  # 阿里仓库 √
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226122023790-1347856469.png)

- 补充：修改国内源的方法 (可略过)

> - 修改`daemon.json`文件，如果没有的话创建一个使用`vim daemon.json`把文件清空后添加以下配置

```shell
# 进入目录
cd /etc/docker

# 编辑配置 
vim daemon.json
```

> - 配置内容：

```json
{
    "registry-mirrors": ["https://registry.docker-cn.com","https://pee6w651.mirror.aliyuncs.com"],
    "live-restore": true
}
```

- 补充：docker国内源说明 (可略过)

> - docker 官方中国区
>
> > [https://registry.docker-cn.com](https://registry.docker-cn.com/)
>
> - 网易
>
> > [http://hub-mirror.c.163.com](http://hub-mirror.c.163.com/)
>
> - 中国科技大学
>
> > [https://docker.mirrors.ustc.edu.cn](https://docker.mirrors.ustc.edu.cn/)
>
> - 阿里云
>
> > [https://pee6w651.mirror.aliyuncs.com](https://pee6w651.mirror.aliyuncs.com/)

### Step2.3 选择docker版本并安装

- 查看所有可用版本有哪些

```shell
yum list docker-ce --showduplicates | sort -r
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226122215264-1296911342.png)

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226122225785-560814338.png)

- 选择1个版本并安装

> ```
> yum install docker-ce-版本号
> 
> # 安装最新版本
> # yum install docker-ce
> ```

```shell
# 默认安装的是最高版本 25.0.3-1.el7
yum -y install docker-ce-25.0.3-1.el7
    注：版本号是 25.0.3-1.el7， 而非 3:25.0.3-1.el7

# 如下指令，可暂忽略
# yum -y install docker-ce-cli:
# yum -y install containerd.io
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226122733789-271197213.png)

```shell
docker version
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226122915264-1849552507.png)

## Step3 启动 Docker

```shell
# 启动 docker
systemctl start docker

# 设置为开机启动
systemctl enable docker
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226123128901-303147209.png)

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226123114955-379705434.png)

```shell
# 查看docker进程的运行状态
systemctl status docker
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226124103373-1789202366.png)

## Step4 查验

```shell
# 查看镜像 （依旧存在）
docker images
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226123748019-1876554058.png)

```shell
# 查看运行的容器
docker ps
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226123210139-312860325.png)

```shell
# 再次查看版本
docker version
```

![img](/img-post/开发/Docker/CentOS 7 升级 Docker.assets/1173617-20240226123257424-449961744.png)

```shell
# 查看 docker 信息
# docker info
Client: Docker Engine - Community
 Version:    25.0.3
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.12.1
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.24.5
    Path:     /usr/libexec/docker/cli-plugins/docker-compose

Server:
 Containers: 17
  Running: 6
  Paused: 0
  Stopped: 11
 Images: 16
 Server Version: 25.0.3
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: ae07eda36dd25f8a1b98dfbf587313b99c0190bb
 runc version: v1.1.12-0-g51d5e94
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
 Kernel Version: 3.10.0-957.21.3.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 1.795GiB
 Name: iZ2vc3en6658r8vwdlz5s3Z
 ID: 95c8a460-14cc-49cf-ac8e-37ccb6b19679
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
```

查看原有容器

docker ps -a



注意：如果启动容器报错

报错内容：Error response from daemon: unknown or invalid runtime name: docker-runc

备份容器信息

cp -r /var/lib/docker/containers/ /var/lib/docker/containers_backup

更改/var/lib/docker/containers目录中的文件参数，把docker-runc替换为runc

grep -rl 'docker-runc' /var/lib/docker/containers/ | xargs sed -i 's/docker-runc/runc/g'

重启Docker

systemctl restart docker

再次启动容器成功。



https://www.cnblogs.com/johnnyzen/p/18034076

https://blog.csdn.net/zhaoyu008/article/details/135901270