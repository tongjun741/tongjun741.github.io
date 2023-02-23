---
tags:
    - 其它
    - Git
    - gitlab
---

Gitlab 自带一个轻量级的 CI [gitlab-ci](https://docs.gitlab.com/ce/ci/)，轻便好用，奈何我们的服务器资源有限，配置不是很高，一直将 gitlab runner 和 gitlab 放在同一台服务器上，每次大规模运行 CI 的时候总是煎熬。经常会因为 CI 负载太高导致 gitlab 暂时不可访问，或者构建产生的容器太多占满了磁盘的空间，苦不堪言。

而[阿里云 kubernetes serverless](https://help.aliyun.com/document_detail/86366.html)的诞生非常好的弥补了这个短板，它非常契合 CI 的这种需求。本文我们分享下我们在 gitlab ci 的实践，使用阿里云 kubernetes serverless 跑 CI 任务。

## gitlab runner 配置

我们是将[gitlab-ce omnibus](https://about.gitlab.com/install/) 安装包和[gitlab-runner](https://docs.gitlab.com/runner/install/index.html)安装到同一台阿里云 ECS 服务器上。运行 gitlab-runner 的注册命令，添加一个[k8s](https://docs.gitlab.com/runner/executors/kubernetes.html)的 executor:

```sh
# sudo gitlab-runner register
Runtime platform                                    arch=amd64 os=linux pid=2934 revision=8fa89735 version=13.6.0
Running in system-mode.

Enter the GitLab instance URL (for example, https://gitlab.com/):
https://mygitlab.domain.com   # <===== 这里输入你的gitlab访问地址
Enter the registration token:
a1b2c3...  # <===== 这里的token在gitlab admin页面runners配置页面下，复制出来即可
Enter a description for the runner:
[gitlab]: k8s # <==== 这里输入你想写的描述，默认gitlab
Enter tags for the runner (comma-separated):
docker,k8s  # <===== 这里输入tag，逗号分隔，用于匹配.gitlab-ci.yml中的tags选择
Registering runner... succeeded                     runner=DRN6fxRz
Enter an executor: custom, docker, shell, ssh, virtualbox, kubernetes, docker-ssh, parallels, docker+machine, docker-ssh+machine:
kubernetes  # <==== 这里我们要添加k8s serverless集群，当然是k8s
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
```

注册成功在 gitlab admin 页面应该就能看到新的 runner 已经添加了(刚添加进去是`locked`状态，在右边的 edit 按钮打开设置页面，激活即可使用)

![gitlab admins runners页面](/img-post/开发/其它/Git/gitlab/Gitlab CI 使用阿里云 kubernetes serverless.assets/gitlab-ci-use-aliyun-k8s-serverless-1.png)

打开`/etc/gitlab-runner/config.toml`文件，可以看到生成的模板类似于下面:

```toml
[[runners]]
  name = "gitlab-k8s"
  url = "https://mygitlab.domain.com"
  token = "your_token"
  executor = "kubernetes"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.kubernetes]
    host = ""
    bearer_token_overwrite_allowed = false
    image = ""
    namespace = ""
    namespace_overwrite_allowed = ""
    privileged = false
    service_account_overwrite_allowed = ""
    pod_annotations_overwrite_allowed = ""
    [runners.kubernetes.affinity]
    [runners.kubernetes.pod_security_context]
    [runners.kubernetes.volumes]
```

到这里基本的 gitlab runner 配置就完成了(后续一些优化配置见后文)

## 开通与配置阿里云 kubernetes serverless

开通阿里云 kubernetes serverless 是免费的，只收取 ECI 实例的费用。有关 ECI 实例的计费参考[计费概述](https://help.aliyun.com/document_detail/89142.html)

在阿里云的控制台上找到 k8s serverless 的入口创建集群就行了:

![阿里云kubernetes serverless创建集群](/img-post/开发/其它/Git/gitlab/Gitlab CI 使用阿里云 kubernetes serverless.assets/gitlab-ci-use-aliyun-k8s-serverless-2.png)

等待集群创建成功，转到`连接信息`页面，复制下来 kubectl 的配置内容:

![kubectl配置内容](/img-post/开发/其它/Git/gitlab/Gitlab CI 使用阿里云 kubernetes serverless.assets/gitlab-ci-use-aliyun-k8s-serverless-3.png)

在 gitlab-runner 服务器上[安装 kubectl](https://kubernetes.io/zh/docs/tasks/tools/install-kubectl/)

将`连接信息`复制到`gitlab-runner`用户的 kubectl 配置中(因为 gitlab-runner 服务使用`gitlab-runner`这个用户运行，所以必须配置成这个用户的 kubectl 配置):

```sh
# 切换成 gitlab-runner 用户，后续操作使用 gitlab-runner 身份
# sudo -iu gitlab-runner
$ mkdir -p ~/.kube
$ vim ~/.kube/config  # <==== 将连接信息复制到这个文件中
```

检查 k8s serverless 集群连接，正常的话大概像这样:

```sh
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"19", GitVersion:"v1.19.4", GitCommit:"d360454c9bcd1634cf4cc52d1867af5491dc9c5f", GitTreeState:"clean", BuildDate:"2020-11-11T13:17:17Z", GoVersion:"go1.15.2", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"18+", GitVersion:"v1.18.8-aliyun.1", GitCommit:"cff3030", GitTreeState:"", BuildDate:"2020-11-19T07:19:32Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/amd64"}
```

说明配置已经 OK 了，此时跑个 CI 任务看看。

> 注意需要运行能跑在 docker 的 CI 任务，可以参考之前的文章[耗时三天，我将 Gitlab CI 由 shell executor 平滑迁移 Docker 环境](https://blog.dteam.top/posts/2019-11/gitlab_ci_migrate_to_docker.html))

![gitlab ci终端输出](/img-post/开发/其它/Git/gitlab/Gitlab CI 使用阿里云 kubernetes serverless.assets/gitlab-ci-use-aliyun-k8s-serverless-4.png)

![k8s serverless页面输出](/img-post/开发/其它/Git/gitlab/Gitlab CI 使用阿里云 kubernetes serverless.assets/gitlab-ci-use-aliyun-k8s-serverless-5.png)

到这里基本的流程已经实现了。

## TIPS

为了节省构建时间以及一些费用，还需要一些优化，这里提供一些优化方案供参考

### 使用 OSS 存储构建缓存

目前 k8s serverless 集群支持 OSS 做 PVC，因此可以考虑将构建缓存放在 OSS 上加速构建过程。在控制台或者 yaml 文件中创建 OSS 的 PVC 即可，我这里的使用的配置如下:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  labels:
    alicloud-pvname: gitlab-cache-pv
  name: gitlab-cache-pv
spec:
  accessModes:
    - ReadWriteMany
  capacity:
    storage: 50Gi
  claimRef:
    apiVersion: v1
    kind: PersistentVolumeClaim
    name: gitlab-cache-pvc
    namespace: default
  flexVolume:
    driver: alicloud/oss
    options:
      akId: 你的AK_ID
      akSecret: 你的AK_SECRET
      bucket: 你的bucket
      otherOpts: "-o max_stat_cache_size=0 -o allow_other"
      # 使用的域名参考OSS文档: https://help.aliyun.com/document_detail/31837.html,我的环境是VPC
      url: vpc100-oss-cn-hangzhou.aliyuncs.com
  persistentVolumeReclaimPolicy: Retain
  storageClassName: oss

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: gitlab-cache-pvc
  namespace: default
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 50Gi
  selector:
    matchLabels:
      alicloud-pvname: gitlab-cache-pv
  storageClassName: oss
  volumeName: gitlab-cache-pv
kubectl apply -f gitlab-cache.yml
```

直接在阿里云控制台界面上创建是同样的效果:

![gitlab-cache-pv](/img-post/开发/其它/Git/gitlab/Gitlab CI 使用阿里云 kubernetes serverless.assets/gitlab-ci-use-aliyun-k8s-serverless-6.png)

然后在`/etc/gitlab-runner/config.toml`中添加 PVC 的挂载:

```toml
[[runners]]
  # ...
  [runners.kubernetes]
    # ...
    [runners.kubernetes.volumes]
      [[runners.kubernetes.volumes.pvc]]
        name = "gitlab-cache-pvc"
        mount_path = "/cache"
```

这样 [gitlab ci cache](https://docs.gitlab.com/ce/ci/caching/) 就可以存储在 OSS 上了，以后构建会快很多。

### 使用阿里云的容器镜像

k8s serverless 集群使用阿里云的容器镜像，阿里云容器镜像如果没有镜像缓存会直接超时 500 错导致构建直接失败，因此有必要在本地就提前预热，建议将本地使用的 docker 镜像仓库配置为阿里云的`https://pqbap4ya.mirror.aliyuncs.com`

> docker 镜像仓库配置参考: [国内开发资源镜像一览](https://blog.dteam.top/mirrors.html#docker-hub)
>
> 也可以使用[镜像缓存](https://help.aliyun.com/document_detail/141281.html)加速 ECI 实例启动，不过会有额外的费用，请注意。

### 使用 ECI 抢占式实例节省费用

由于 CI 对可用性没有那么高的要求，也不会持续运行太长时间，因此，使用[ECI 抢占式实例](https://help.aliyun.com/document_detail/157759.html)就足够了，可以进一步节省很多成本。

在 gitlab-runner 中使用 ECI 抢占式实例的配置如下:

```toml
[[runners]]
  # ...
  [runners.kubernetes]
    # ...
    [runners.kubernetes.pod_annotations]
      "k8s.aliyun.com/eci-spot-strategy" = "SpotAsPriceGo"
```

> 更多 annotations 配置参考: [使用抢占式实例](https://help.aliyun.com/document_detail/165053.html)

### 限制构建环境使用的资源配置

ECI 按照配置规格按量付费，因此可以限制资源使用来达到节省费用的目的。有两种方式可以限制使用的 ECI 规格：

- 声明使用的资源(CPU, 内存等)，ECI 会自动适配一个最接近的实例
- 使用 annotations 直接声明具体要用的实例规格

> 这部分具体的详情规则参见官方文档: [实例概述](https://help.aliyun.com/document_detail/114665.html)

我这边使用的是声明资源，由 ECI 自动适配实例规格的方案。gitlab-runner 配置中可以限定 helper，build，service 容器实例的规格，我的配置参考如下:

```toml
[[runners]]
  # ...
  [runners.kubernetes]
    # ...
    cpu_request = "2"
    memory_request = "6Gi"
    service_cpu_request = "1"
    service_memory_request = "1Gi"
    helper_cpu_request = "250m"
    helper_memory_request = "512Mi"
```

限定 build 容器使用 2 核 6G，service 容器使用 1 核 1G，helper 容器使用 1/4 核 512M，最终 ECI 自动适配了一个 4 核 8G 的实例（见上图）

也可以通过 annotations 的方式直接指明要用的实例，参考[指定 ECS 规格创建 ECI](https://help.aliyun.com/document_detail/114664.html)

### k8s 中 service 网络组建的问题

在 docker executor 中，service 是通过 docker container link 的方式组建网络的(参考 gitlab ci 官方文档: [How services are linked to the job](https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#how-services-are-linked-to-the-job))，直观的表现就是**要通过 container_name 作为域名访问到 service**。但是在 k8s 网络中，service 网络组建是直接通过`host`的方式连接容器网络的，直观的表现就是**可以通过`127.0.0.1`直接访问到 service**，为了同时适配这两种可能的场景，需要在`.gitlab-ci.yml`的配置中作下简单的适配，参考配置（主要参考来自于官方 issue: [#2229](https://gitlab.com/gitlab-org/gitlab-runner/-/issues/2229)）:

```yml
test:
  stage: test
  image: adoptopenjdk/openjdk8-openj9:alpine-slim
  services:
    - name: postgres:10-alpine
      alias: db
  variables:
    POSTGRES_DB: mydb_test
    POSTGRES_USER: my_user
    POSTGRES_PASSWORD: my_pass
  script:
    - DB_HOST="${CI_SERVICE_HOST:-db}"
    # 你的测试需要支持从环境变量获取数据库连接方式
    - export dataSource_url="jdbc:postgresql://${DB_HOST}:5432/${POSTGRES_DB}?useUnicode=true&characterEncoding=utf8"
    - ./gradlew -Dorg.gradle.daemon=false -Dfile.encoding=UTF-8 check
  artifacts:
    expose_as: "test reports"
    paths:
      - build/reports/tests/
    reports:
      junit: "build/test-results/**/*.xml"
    expire_in: 1 day
    when: always
  dependencies: []
  rules:
    - if: $CI_COMMIT_BRANCH
  tags:
    - docker
```

这里我们通过额外注入一个`CI_SERVICE_HOST`环境变量控制 service 的实际连接地址，然后我们在 gitlab-runner 的配置中注入这个环境变量:

```toml
[[runners]]
  name = "k8s"
  executor = "kubernetes"
  # ...
  environment = ["CI_SERVICE_HOST=127.0.0.1"]
  # ...
```

这样无论 CI 是运行在 docker executor 还是 k8s executor 都可以正常通过测试了。

### pod 访问外网环境

构建过程中可能需要从外网下载依赖，由于 pod 直接使用 VPC 网络环境，在不想额外增加成本的情况下，可以参考这篇文章使用外网 ECS 解决网络访问: [阿里云 VPC 环境内网服务器如何访问外网](https://blog.dteam.top/posts/2018-08/阿里云vpc环境内网服务器如何访问外网.html)

## 小结

本文我们详细总结了使用阿里云 k8s serverless 服务对接 gitlab ci 的方案，以及我们的一些实践。



https://blog.dteam.top/posts/2020-11/gitlab-ci-use-aliyun-k8s-serverless.html