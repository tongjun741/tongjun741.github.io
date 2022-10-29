---
tags:
    - 其它
    - Jira及Confluence
---

## 一、 说明

### 1.1 素材

本文采用素材如下：

- Docker镜像 [Github链接](https://github.com/cptactionhank)
- 破解工具 [Gitee链接](https://gitee.com/pengzhile/atlassian-agent)

采用以上工具，理论上可以破解**几乎**全部版本。

## 二、操作系统准备

操作系统为Cent OS 7，防火墙需要允许外部访问9005和9006端口。

挂载数据盘并安装相应软件：

```
sudo su
mkdir /mnt/data
mount -o rw,nouuid /dev/sdb2 /mnt/data/
yum install wget docker mysql java -y
systemctl enable docker
systemctl start docker
```

## 三、安装 Mysql 5.7

```javascript
# 启动容器mysql
docker run --name mysql \
    --restart always \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=123456 \
    -v data_mysql_vol:/var/lib/mysql \
    -v conf_mysql_vol:/etc/mysql/conf.d \
    -v data_backup_vol:/backup \
    -d mysql:5.7
```

### 1.1 配置数据库

MySQL所使用的配置文件my.cnf核心参数：

```
vim /var/lib/docker/volumes/conf_mysql_vol/_data/my.cnf
```



```javascript
[client]
default-character-set = utf8

[mysql]
default-character-set = utf8

[mysqld]
character_set_server = utf8
collation-server = utf8_bin
transaction_isolation = READ-COMMITTED

```

需要注意的是，Confluence需要使用utf8_bin，并将事务隔离策略设为READ-COMMITTED。

```
docker restart mysql
```

### 1.2 创建表及用户

```
mysql -h 127.0.0.1 -p
```



```javascript
--创建jira数据库及用户
drop database jira;
create database jira character set 'UTF8';
create user jira identified by 'jira';
grant all privileges on `jira`.* to 'jira'@'172.%' identified by 'jira' with grant option;
grant all privileges on `jira`.* to 'jira'@'localhost' identified by 'jira' with grant option;
flush privileges;

--创建confluence数据库及用户
drop database confluence;
create database confluence character set 'UTF8';
create user confluence identified by 'confluence';
grant all privileges on `confluence`.* to 'confluence'@'%' identified by 'confluence' with grant option;
grant all privileges on `confluence`.* to 'confluence'@'localhost' identified by 'confluence' with grant option;
flush privileges;
--设置confluence字符集
alter database confluence character set utf8 collate utf8_bin;
-- confluence要求设置事务级别为READ-COMMITTED
set global tx_isolation='READ-COMMITTED';
--set session transaction isolation level read committed;
show variables like 'tx%';
```

## 二、 安装 JIRA(7.6.2)

JIRA 是一个缺陷跟踪管理系统，为针对缺陷管理、任务追踪和项目管理的商业性应用软件，开发者是澳大利亚的Atlassian。JIRA这个名字并不是一个缩写，而是截取自“Gojira”，日文的哥斯拉发音。 [官网](https://www.atlassian.com/software/jira)

### 2.1 制作Docker破解容器

编写`Dockerfile`文件：

```dockerfile
FROM cptactionhank/atlassian-jira-software:7.6.2

USER root

# 将代理破解包加入容器
COPY "atlassian-agent.jar" /opt/atlassian/jira/

# 设置启动加载代理包
RUN echo 'export CATALINA_OPTS="-javaagent:/opt/atlassian/jira/atlassian-agent.jar ${CATALINA_OPTS}"' >> /opt/atlassian/jira/bin/setenv.sh
```

### 2.2 下载破解文件

在gitee 中下载编译好的即可，放置在Dockerfile同目录下

```css
- JIRA
  --Dockerfile
  --atlassian-agent.jar
```

### 2.3 构建镜像

```shell
docker build -t yuexuan/jira:latest .
```

结果如下：

```rust
Status: Downloaded newer image for docker.io/cptactionhank/atlassian-jira-software:7.6.2
 ---> 166e87d8251a
Step 2/4 : USER root
 ---> Running in b46d95e269aa
 ---> 6c8f92d74a83
Removing intermediate container b46d95e269aa
Step 3/4 : COPY "atlassian-agent.jar" /opt/atlassian/jira/
 ---> 093511981469
Removing intermediate container 137dc681e7cf
Step 4/4 : RUN echo 'export CATALINA_OPTS="-javaagent:/opt/atlassian/jira/atlassian-agent.jar ${CATALINA_OPTS}"' >> /opt/atlassian/jira/bin/setenv.sh
 ---> Running in cb07746a5092

 ---> 641dd7d837e1
Removing intermediate container cb07746a5092
Successfully built 641dd7d837e1
```

### 2.4 启动容器

```shell
docker run --name jira \
    --restart always \
    --link mysql:mysql \
    -p 9005:8080 \
    -v data_jira_var:/var/atlassian/jira \
    -v data_jira_opt:/opt/atlassian/jira \
    -d yuexuan/jira:latest
```

准备备份好的数据：

```
cd /var/lib/docker/volumes/data_jira_var/_data/
cp /mnt/data/2021-Apr-01--0800.zip ./
```



### 2.5 访问 jira

访问 IP:9005，选择语言并选择手动配置

![image-20200510115202911](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/115203-136565-20200915191546759.png)

使用其它数据库：

![image-20210414102827891](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/image-20210414102827891.png)

设置属性

![image-20200510115613978](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/115614-191487-20200915191546773.png)

### 2.6 破解

![image-20200510115713127](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/115713-329032-20200915191546768.png)

1. 复制服务器ID: **BRQE-TEN6-TLYV-KFMI**
2. 在本地存放`atlassian-agent.jar`的目录下执行命令，生成许可证：
3. 需替换邮箱（test@test.com）、名称（BAT）、访问地址（[http://192.168.0.89）、服务器ID（BY9B-GWD1-1C78-K2DE）为你的信息](http://192.168.0.89）、服务器ID（BY9B-GWD1-1C78-K2DE）为你的信息/)

```shell
java -jar atlassian-agent.jar \
   -m test@test.com -n BAT \
  -p jira -o http://192.168.0.89 \
  -s BIRH-X9JN-X0H9-5S6J
```

例如我的信息如下，生成许可证：

```bash
====================================================
=======        Atlassian Crack Agent         =======
=======           https://zhile.io           =======
=======          QQ Group: 30347511          =======
====================================================

Your license code(Don't copy this line!!!): 

AAABoQ0ODAoPeJx9ktFPqzAUxt/5K0h8LpaazbmE5CrUhAhMB5rcx46dbTWskNMynX+9Hcy467wkv
LSn5/vO+X1c5K1yU7F3fer6bEpHU8rcMC9cRhl1XiUKr8F62ZbGOxyIrlfmTSB4ojRyB4HBFpxUS
GVACVUCf28k7iNhIGB0ckPotf2GdLJ2uwCcrZ41oA6I37+1AiIEK4q9Qw64A4yj4G7+xEnBszEpk
r8v5OE+jZ1ElqA02GoSRznPSOKPJjd0cjWi7HrsO2GtjB2X2zGr4E2o9YdQ/uSPz8ZeWW97w9N1+
E5UrTCyVsFKVBqcxxbLjdBwXItRQkfEp1/Gxb6BTGwhCGdpyudhfJs4awRQm7ppAP/R7swG+rr6G
ZNjw+9wz16feg9YRaBLlE2357Oq5FYaWLpV3+Au9u7GmEZPLy8/NrICT9ZDMeZG4CGtHtiReOd4d
1s4M1wLJXXP9CBrVTsxyx/trVrrLgobXvBbgAhd60/+ebv4XuEML1/KfrcsidO44NHQ/D9/uFOG3
XWDUsN57b9eR44vNpNDgX0C7gEsJDAsAhQGL/A02nteG056fiVCh12XIgz+KwIUG3z2e35ugE7Pc
N6ZMj+Aum9LTK8=X02k4
```

回到初始化页面选择导入到当前JIRA：

```
/secure/SetupApplicationProperties!default.jspa
```

![image-20210414103918194](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/image-20210414103918194.png)

输入文件名与密钥即可完成导入：

![image-20210414104011265](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/image-20210414104011265.png)

导入完成后需要把附件文件夹拷贝到指定目录并重启jira：

```
rm -rf /var/lib/docker/volumes/data_jira_var/_data/data/attachments
mv  /mnt/data/JIRA/attachments/  /var/lib/docker/volumes/data_jira_var/_data/data
docker restart jira
```



如果不需要导入数据可以直接将生成的许可证复制到页面，完成破解。

![image-20200510120630388](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/120631-630766-20200915191546761.png)

查看许可结果

![image-20200510154122566](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/154123-138397-20200915191546777.png)

## 三、安装 Confluence(6.6.2)

Atlassian Confluence（简称Confluence）是一个专业的wiki程序。它是一个知识管理的工具，通过它可以实现团队成员之间的协作和知识共享。[官网](https://www.atlassian.com/software/confluence)

### 3.1 编写Dockerfile文件：

```dockerfile
FROM cptactionhank/atlassian-confluence:6.6.2

USER root

# 将代理破解包加入容器
COPY "atlassian-agent.jar" /opt/atlassian/confluence/

# 设置启动加载代理包
RUN echo 'export CATALINA_OPTS="-javaagent:/opt/atlassian/confluence/atlassian-agent.jar ${CATALINA_OPTS}"' >> /opt/atlassian/confluence/bin/setenv.sh
```

### 3.2 下载破解文件

在gitee 中下载编译好的即可，放置在Dockerfile同目录下

```css
- Confluence
  --Dockerfile
  --atlassian-agent.jar
```

### 3.3 构建镜像

```bash
docker build -t yuexuan/confluence:latest .

Sending build context to Docker daemon 976.9 kB
Step 1/4 : FROM cptactionhank/atlassian-confluence:6.6.2
Trying to pull repository docker.io/cptactionhank/atlassian-confluence ...
6.6.2: Pulling from docker.io/cptactionhank/atlassian-confluence
ff3a5c916c92: Pull complete
5de5f69f42d7: Pull complete
fd869c8b9b59: Pull complete
b8c8b57b8a0a: Pull complete
b0ca65a3f7aa: Pull complete
Digest: sha256:e131d45fc9d91dac8c36a63873ec088ebe3418baf10936b591749b530c8c9d82
Status: Downloaded newer image for docker.io/cptactionhank/atlassian-confluence:6.6.2
 ---> 540b703e337b
Step 2/4 : USER root
 ---> Running in 2cb7788ad08d
 ---> c76181cf93ad
Removing intermediate container 2cb7788ad08d
Step 3/4 : COPY "atlassian-agent.jar" /opt/atlassian/confluence/
 ---> 4b89b309ee8e
Removing intermediate container 99e47cd8e062
Step 4/4 : RUN echo 'export CATALINA_OPTS="-javaagent:/opt/atlassian/confluence/atlassian-agent.jar ${CATALINA_OPTS}"' >> /opt/atlassian/confluence/bin/setenv.sh
 ---> Running in 6be55e71caaf

 ---> 98a871deaf75
Removing intermediate container 6be55e71caaf
Successfully built 98a871deaf75
```

### 3.4 启动容器

```shell
docker run -d --name wiki \
  --restart always \
    --link mysql:mysql \
  -p 9006:8090 \
  -e TZ="Asia/Shanghai" \
  -v data_wiki_var:/var/atlassian/confluence \
  yuexuan/confluence:latest
```

### 3.5 访问 confluence

访问 IP:9006，参照JIRA的安装流程，进行操作。可在引导过程中，与之前安装的JIRA进行绑定关联。

![image-20200510152424850](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/152425-514599-20200915191546770.png)

我们就选择一个应用吧

![image-20200510152518219](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/152519-264644-20200915191546776.png)

### 3.6 破解

生成confluence许可命令参照如下：

```shell
# 设置产品类型：-p conf， 详情可执行：java -jar atlassian-agent.jar 
java -jar atlassian-agent.jar -m test@test.com -n BAT -p conf -o http://192.168.0.89 -s BPRN-WZKW-PYC8-I9FN

====================================================
=======        Atlassian Crack Agent         =======
=======           https://zhile.io           =======
=======          QQ Group: 30347511          =======
====================================================

Your license code(Don't copy this line!!!): 

AAABXQ0ODAoPeJx1kV9vgjAUxd/7KUj2XG1R5p+EZArEmYEsotteK7tqEyikLW7s069UzJJlS/rQn
HtPf/fc3mWNcBLWOpQ41JuT6dylTpDtHJe4BCWMCw2CiRyiz5rLNmQafJdMZ5hMzEExz0Gof4ohq
FzyWvNK+HtR8JJreHeKq8U5tM5Z61rNh8OvMy9gwCsUVEKzXG9YCf5ysUMZyAvIdegvR49jHNL0F
a+25A0/Be4K5ZU4DjZNeQCZHvcKpPIxRak8McEVs9QOYN7vOosGTIpBXpXS1MRJddcbMDJBC/+Di
dMXE3T6QN17W7aIPuOurcEOFqRJEm2D9SJGgQQL6pO7BBMPU3Jbixk8XodZtMEx9aYzMht7hI4mH
jKS/4dscWYcfgFfywZQdGFFc41yZIUC9NzI/MwU/AZmzeFn19Zq38o0kxpkb7aScbIARKfavv47X
sz6Oq/7DWevrTUwLgIVAIEyoNFjmUFyTJOVUzmxTJTM14S8AhUAkaRbRjdl4D9MZtO6l5nCHcR2B
80=X02h9
```

![image-20200510152622252](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/152623-217452-20200915191546780.png)

选择单机模式，并设置数据库

![微信图片_20200510204736](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/204802-411920-20200915191546782.png)

### 3.7 配置 confluence

我们做个示范站点

![image-20200510153008698](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/153009-849739-20200915191546785.png)

配置用户管理，这里我们选择之前创建好的 jira

![image-20200510153055878](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/153056-726139-20200915191546794.png)

配置连接信息

![image-20200510153216666](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/153217-339170-20200915191546790.png)

同步数据

![image-20200510153306774](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/153307-502952-20200915191546796.png)

大功告成

![image-20200510153354988](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/153356-758147-20200915191546797.png)

登陆查看授权情况

![image-20200510154035663](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/154036-494460-20200915191546801.png)

导入已有的备份数据：

```
cp /mnt/data/xmlexport-20200813-20565-4.zip /var/lib/docker/volumes/data_wiki_var/_data/restore/
```



![image-20210414110108463](/img-post/开发/其它/Jira及Confluence/Docker部署JIRA和Confluence-破解版.assets/image-20210414110108463.png)

重置与jira的连接：

https://www.jianshu.com/p/0191c673217f

还原附件：

```
rm -rf /var/lib/docker/volumes/data_wiki_var/_data/attachments
mv /mnt/data/WIKI-attachments /var/lib/docker/volumes/data_wiki_var/_data/
docker restart wiki
```





## 四、乱码问题

在我们正常安装之后，中文可能会有乱码，我们修改一下连接字符串，在 confluence 的家目录下面，有一个配置文件`confluence.cfg.xml`，找到`hibernate.connection.url`，在数据库字符串后面加上如下字符，整体结果如下：

```xml
jdbc:mysql://172.17.64.10/confdb?useUnicode=true&characterEncoding=utf8
```

记住，里面的`amp;`不要省略。

如果可以的话，把数据库的字符串改成`utf8mb4`

https://confluence.atlassian.com/doc/database-setup-for-mysql-128747.html

还有一个文档说字符串改成`utf8`，不是`utf8mb4`，具体区别我也不知道，大家可以去测试一下

https://www.cwiki.us/display/CONFLUENCEWIKI/Database+Setup+For+MySQL



来源：

https://blog.51cto.com/wzlinux/2494063