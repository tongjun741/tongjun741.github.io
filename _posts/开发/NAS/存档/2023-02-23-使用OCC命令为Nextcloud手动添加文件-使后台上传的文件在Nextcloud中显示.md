---
tags:
    - NAS
    - 存档
---

使用OCC命令为Nextcloud手动添加文件-使后台上传的文件在Nextcloud中显示

有时候，直接通过Web页面上传文件并不那么方便，于是有的朋友就直接把文件上传到服务器里，然后拷贝到data目录下，打开ownCloud，却还是之前的文件。

这是因为虽然上传了文件，但是ownCloud/Nextcloud的数据库里并没有这个文件的信息。文件信息都被存储在数据库的oc_filecache表中。

![](/img-post/开发/NAS/存档/使用OCC命令为Nextcloud手动添加文件-使后台上传的文件在Nextcloud中显示.assets/e2c8ed1fb1384654809d7d9da17421e9.png)

使用OCC命令更新文件索引。

occ有三个用于管理Nextcloud中文件的命令：

files
 files:cleanup              #清楚文件缓存
 files:scan                 #重新扫描文件系统
 files:transfer-ownership   #将所有文件和文件夹都移动到另一个文件夹

我们需要使用

files:scan 

 来扫描新文件。

  格式:
  files:scan [-p|--path="..."] [-q|--quiet] [-v|vv|vvv --verbose] [--all]
  [user_id1] ... [user_idN]

参数:
  user_id               #扫描所指定的用户（一个或多个，多个用户ID之间要使用空格分开）的所有文件

选项:
  --path                #限制扫描路径
  --all                 #扫描所有已知用户的所有文件
  --quiet               #不输出统计信息
  --verbose             #在扫描过程中显示正在处理的文件和目录
  --unscanned           #仅扫描以前未扫描过的文件

以下是一个具体的命令示例：

sudo -u www-data php occ files:scan --all   #扫描所有用户的所有文件

执行命令后未进行扫描并列出扫描信息。

![](/img-post/开发/NAS/存档/使用OCC命令为Nextcloud手动添加文件-使后台上传的文件在Nextcloud中显示.assets/8fb39aa88dd74dd5b43179c9cfaadc67.png)

如果不想显示扫描信息，可以在后面加上

--quiet

 ，如下：

sudo -u www-data php occ files:scan --all --quiet

指定扫描位置

总是扫描全部信息并不是那么有必要，还会白白消耗服务器资源。

指定扫描的用户

列出所有用户：

sudo -u www-data php occ user:list

![](/img-post/开发/NAS/存档/使用OCC命令为Nextcloud手动添加文件-使后台上传的文件在Nextcloud中显示.assets/a2b5427c2c3b4b2b9346911153b5314c.png)

为用户ChengYe扫描文件：

sudo -u www-data php occ files:scan ChengYe


![](/img-post/开发/NAS/存档/使用OCC命令为Nextcloud手动添加文件-使后台上传的文件在Nextcloud中显示.assets/aaed279d35714954a34799fd1fbc3422.png)

指定扫描目录

当使用

--path

 选项时，该路径必须包含以下部分：

"user_id/files/path"
  或
"user_id/files/mount_name"
  或
"user_id/files/mount_name/path"

其中，

/files/

是必须要加上的，不可忽略。

示例：

sudo -u www-data php occ files:scan --path="/ChengYe/files/Photos" #指向用户ChengYe的Photos文件夹

![](/img-post/开发/NAS/存档/使用OCC命令为Nextcloud手动添加文件-使后台上传的文件在Nextcloud中显示.assets/991fc3e859c746d7a642329a1b741289.png)

 



https://www.orgleaf.com/2400.html





登陆nextcloud容器中：



sudo -u www-data php occ files:scan --all   #扫描所有用户的所有文件



如果显示没发现sudo的话   安装即可  安装步骤和安装vim是一样的：“apt-get update” 更新完毕后：“apt-get install sudo”



也可以在容器外部同步



docker exec -it nextcloud /bin/bash -c "sudo -u www-data php occ files:scan --all"



同步完成后就会发现该有的有了 不该有的没了 

————————————————

版权声明：本文为CSDN博主「BiuBiuBiu___」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/BiuBiuBiu___/article/details/90600264



Docker版NextCloud文件手动扫描同步 定时扫描

2018-11-22 10:37:38 芽孢八叠球菌 阅读数 1578更多

分类专栏： Docker

版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

本文链接：https://blog.csdn.net/u010457406/article/details/84335143

由于使用NextCloud的上传太慢了，我直接将文件移动至nextcloud的文件目录/data/用户名/files中，结果nextcloud并不能显示出来手动拷贝的文件，本文详细说明了docker版本的nextcloud如何手动扫描文件。

1. NextCloud的docker启动脚本

#!/bin/bash
docker run -d \
-v /home/docker/nextcloud/data:/var/www/html/data \
-v /home/movies:/var/www/html/data/用户名/files/Movies \
-v /home/docker/nextcloud/custom-apps:/var/www/html/custom_apps \
-v /home/docker/nextcloud/config:/var/www/html/config \
-v /home/docker/nextcloud/config/passwd:/etc/passwd \
-p 80:80 \
--link mysql:mysql \
--name nextcloud \
--restart=always \
helsing/nextcloud


- 1

- 2

- 3

- 4

- 5

- 6

- 7

- 8

- 9

- 10

- 11

- 12

注意：

这里的passwd文件要映射出来，因为官方docker镜像里，www-data用户是禁止使用bash的，需要手动修改一下：

vi passwd


- 1

修改www-data用户的nologin为/bin/bash

www-data:x:33:33:www-data:/var/www:/bin/bash


- 1

2. 容器内执行方式

进入容器内

docker exec -it nextcloud /bin/bash


- 1

手动扫描文件

su - www-data -c 'php /var/www/html/occ files:scan --all'


- 1

一般默认安装occ都在我上边写的那个路径下，如果没有，请自行搜索位置

正常返回结果

Starting scan for user 1 out of 1 (xxx)

+---------+-------+--------------+
| Folders | Files | Elapsed time |
+---------+-------+--------------+
| 70      | 8320  | 00:03:10     |
+---------+-------+--------------+


- 1

- 2

- 3

- 4

- 5

- 6

- 7

可以看到，用时还是比较长的，所以可以自行添加一些参数，比如指定扫描用户、目录、只扫描未扫描过的文件等。

occ扫描参数说明

格式: files:scan [-p|--path="..."] [-q|--quiet] [-v|vv|vvv --verbose] [--all] [user_id1] ... [user_idN]

参数: 
user_id #扫描所指定的用户（一个或多个，多个用户ID之间要使用空格分开）的所有文件

选项: 
--path #限制扫描路径,该路径必须包含以下部分："user_id/files/path"
--all #扫描所有已知用户的所有文件 
--quiet #不输出统计信息 
--verbose #在扫描过程中显示正在处理的文件和目录 
--unscanned #仅扫描以前未扫描过的文件


- 1

- 2

- 3

- 4

- 5

- 6

- 7

- 8

- 9

- 10

- 11

其他可能用到的

php occ user:list #列出所有用户


- 1

3. 容器外手动/定时执行

容器外执行occ的脚本scanFiles.sh内容

#/bin/bash
#可以根据自己的需求更改参数，比如指定扫描路径，只扫描未扫描过的文件等，参见第二节的参数说明
docker exec -it nextcloud /bin/bash -c "su - www-data -c 'php /var/www/html/occ files:scan --all'"


- 1

- 2

- 3

增加执行权限

chmod +x scanFiles.sh


- 1

- 手动执行

./scanFiles.sh


- 1

- 定时任务

crontab -e


- 1

按i进入编辑模式，插入如下记录

#每天凌晨2点定时occ扫描nextcloud文件更新
0 2 * * *  sh /home/shells/scanFiles.sh


- 1

- 2



