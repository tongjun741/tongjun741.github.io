---
tags:
    - 操作系统
    - Windows
---

相信一提到Rsync你们并不陌生，第一想到的就是他是Linux下一款可以实现远程同步备份以及本地复制的一款工具css

那么，今天给你们演示的并非这款工具，而是和他有着相同功能的工具名字叫CwRsync。node

------

# 什么是[cwRsync](http://www.javashuo.com/link?url=http://lovesoo.org/tag/cwrsync)？

------

[cwRsync](http://www.javashuo.com/link?url=http://lovesoo.org/tag/cwrsync)是Rsync在Windows上的实现版本，Rsync经过使用特定算法的文件传输技术，能够在网络上传输只修改了的文件。linux

[cwRsync](http://www.javashuo.com/link?url=http://lovesoo.org/tag/cwrsync)主要用于Windows上的[远程](http://www.javashuo.com/link?url=http://lovesoo.org/tag/远程)文件同步备份和同步，它包含[Cygwin](http://www.javashuo.com/link?url=http://lovesoo.org/tag/cygwin) DLL和适用[Cygwin](http://www.javashuo.com/link?url=http://lovesoo.org/tag/cygwin)版本的Rsync两部分。web

cwRsync分为[Server](http://www.javashuo.com/link?url=http://lovesoo.org/tag/server)和[Client](http://www.javashuo.com/link?url=http://lovesoo.org/tag/client)，Rsync只能针对在Linux系统服务下进行同步机本地复制，那么CwRsync既能够实如今Windows和Windows之间数据的同步，也可让Windows和Linux之间进行数据的同步。算法

------

目前能找到的最新版本是4.0.1。下载地地：[http://ftp.pconline.com.cn/2292f7e5b926933e6e663935fef60664/pub/download/201010/pconline1477625669671.zip](http://www.javashuo.com/link?url=http://ftp.pconline.com.cn/2292f7e5b926933e6e663935fef60664/pub/download/201010/pconline1477625669671.zip)shell

------

接下来就来演示一下Windows与Windows之间如何进行同步以及备份，首先开启两台Windows服务，这里以Windows Server 2008 R2为例：vim

**环境：**windows

- **主服务器A：192.168.1.10**
- **从服务器B：192.168.1.20**

**提示：服务器之间要可以保证通讯。**安全

------

### 1.主服务器A安装cwRsyncServer_4.0.1_Installer.zip工具，server端包括了client的功能。

### 1.1将软件上传到服务器上

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/539_29b_1a7.png)

### 1.2打开并运行安装该软件

### ![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/540_900_92d.png)

### 点击下一步：

### ![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/541_0fa_70d.png)

### 1.3下一步选择安装目录，点击安装便可！！！

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/542_2f2_22f.png)

### 1.4下一步设置用户名和密码：

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/543_a89_9dc.png)

### 安装进度：

### ![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/544_8cd_26e.png)

### **1.5安装完成以后下面开始配置，打开安装目录下的文件目录：D:\ICW.**

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/545_feb_0ac.png)

------

### 1.6主服务器A进行配置：

### **修改rsyncd.conf配置文件的内容为如下配置:**

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/546_f3b_0c9.png)

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/547_d1c_fce.png)

### 这里要注意的是，若是不写uid=0,和gid=0的话就，在远程链接时就会出现如下的状况：

![cwrsyncserver4](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/548_e65_1f4.JPEG)

### 1.7保存退出，**rsyncd.conf****相关解释：**

------

```
port = 9999 #默认端口是873，作了端口限制的要开启cwRsync所使用的端口。
use chroot = false
strict modes = false
hosts allow = *
log file = rsyncd.log #LOG
pid file = rsyncd.pid
# Module definitions
# Remember cygwin naming conventions ： c：\work becomes /cygwin/c/work
#
[web]
path = /cygdrive/d/web/test #注意格式，这说明是D盘WEB目录下的test目录
read only = true #只读
list = no
auth users = username  #指定用户名, 若是没有这行，则代表是匿名
secrets file=/cygdrive/d/rsyncd.secrets  这里指定了认证文件目录，名字叫 rsyncd.secrets，其内容是txt编辑为 username:123456 前面是用户名，后面是密码
transfer logging = no #是否记录详细的传输状况
use chroot = no        # 不使用chroot
max connections = 4    # 最大链接数为4
pid file = /cygdrive/d/rsyncd.pid 
lock file = /cygdrive/d/rsync.lock
log file = /cygdrive/d/log/rsyncd.log    # 日志记录文件
[web]            # 这里是认证的模块名 client端须要根据此名字进行同步
path = /cygdrive/d/web/test    # 须要作镜像的目录
comment = BACKUP CLIENT IS SOLARIS 8 E250 
ignore errors           # 能够忽略一些无关的IO错误
read only = yes         # 只读
list = no               # 不容许列文件
hosts allow=192.168.0.20         #容许链接IP，不限制则填写 * 
auth users = username      # 认证的用户名，若是没有这行，则代表是匿名
secrets file = /cygdrive/d/rsyncd.secrets    # 认证文件名
```

### 1.8:到这里了就要在D盘下新建一个rsyncdata的目录，这个目录就是指它里面的全部数据同步到另外一台windows的指定目录去的，也就是windows下的rsyncd.conf配置文件的[rsyncdata]模块对应的文件，这里指定了认证文件目录，名字叫 rsyncd.secrets，其内容是txt编辑为 username:123456 前面是用户名，后面是密码

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/549_789_662.png)

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/550_11a_78f.png)

### **1.9新建好以后咱们还须要改一些配置：**

### **还须要修改一下rsyncdata这个目录的一些相关属性信息，右键这个文件选属性：**

------

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/551_8da_4ea.png)

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/552_b9c_a9e.png)

### 1.10.输入完SvcCWRSYNC这个用户名后点“检查名称”就能够匹配上了，再点肯定就OK了。

### 最后再改一下这个文件对这个用户的访问权限：

------

![img](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/553_b7a_95d.png)

### 1.11.选中个人电脑--右键管理---服务和应用--服务，找到RsyncSever，双击--启动，把这个服务器起动起来：

![cwrsyncserver4](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/554_262_a5f.JPEG)

![cwrsyncserver](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/555_cce_82d.JPEG)

> ### 到这里应该是说windows下的就配置完了，可是要注意的是若是你的linux须要用telnet来链接到windows上来的话那windows上的[防火墙](http://www.javashuo.com/link?url=http://www.downcc.com/k/Firewall/)记得要关闭，不然极可能连不上去，也能够在本上的测试一下，出现如下状况就说明能够链接上去了，说明windows服务可用了

### ![cwrsyncserver4](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/556_55e_eca.JPEG)

### 在cmd下输入你windows的本机地址和rsync的监听端口，它的默认监听的端口是873，回车：

![cwrsyncserver](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/557_416_33b.JPEG)![img](CwRsync  Windows与Windows之间同步备份配置详解.assets/558_8b2_abf.JPEG)

### 出现@RSYNCD:30.0这个就说明能够链接上去了。

------

### 2.从服务器client-B安装cwRsync_4.0.1_Installer.zip

------

> **2.1telnet 192.168.1.20 9999 #链接A服务器测试**bash
> 
> **2.2设置客户端密码文件 例如：/cygdrive/d/rsyncd.secrets 内容只要含有密码行 123456 便可,为安全属性改成只读。**

### 特别注意：

> \#从服务器B密码文件存放的位置必定要是NTFS分区;
> 
> \#另外 --password-file=/cygdrive/d/rsyncd.secrets最好是最后面指定：
> 
> Rsync -vzrtopg --progress  --delete username@192.168.1.10::web /cygdrive/d/test --password-file=/cygdrive/d/rsyncd.secrets

### 三、cwRsync数据同步

> 由于只是最简单的数据同步，因此就不使用 ssh 了，直接启动 rsync 服务就能够了。
> 
> 程序代码：
> 
> \#无密码、端口:
> 
> rsync -vzrtopg  --progress  --delete  --port 9999 192.168.1.10::web /cygdrive/d/test
> 
> \#有密码、端口:
> 
> Rsync -vzrtopg --progress  --delete test@192.168.1.10::web /cygdrive/d/test --password-file=/cygdrive/d/rsyncd.secrets

### **注：**

> 1.password-file，你要在指定的目录下定义一个 rsyncd.secrets文件，只要写test这个用户名对应的密码就能够。这里是D盘根目录下的rsyncd.secrets。
> 
> \2. (表示将客户端test目录下文件备份到服务器test模块下。若是将/test/放后面，表示将服务器test模块下目录备份到客户端/test/下。)

### **5****、cwRsync同步常见问题：**

### **错误一：**

> @ERROR: auth failed on module xxxxx
> 
> rsync: connection unexpectedly closed (90 bytes read so far)
> 
> rsync error: error in rsync protocol data stream (code 12) at io.c(150)
> 
> 解决：这是由于密码设置错了，没法登入成功，检查一下rsync.pwd，看客户端服务是否匹配。还有服务器端没启动rsync 服务也会出现这种状况。

### 错误二：

> password file must not be other-accessible
> continuing without password file
> Password:
> 
> 解决：这是由于rsyncd.secrets的权限不对，或存放的位置不是NTFS分区

### 错误三：

> @ERROR: chroot failed
> 
> rsync: connection unexpectedly closed (75 bytes read so far)
> 
> rsync error: error in rsync protocol data stream (code 12) at io.c(150)
> 
> 解决：这是由于你在 rsync.conf 中设置的 path 路径不存在，要新建目录才能开启同步。

### 错误四：

> rsync: failed to connect to 192.168.0.10: No route to host (113)
> 
> rsync error: error in socket IO (code 10) at clientserver.c(104) [receiver=2.6.9]
> 
> 解决：对方没开机、防火墙阻挡、经过的网络上有防火墙阻挡，都有可能。关闭防火墙，其实就是把tcp udp 的端口（默认873）打开。

### Rsync客户端经常使用参数说明：

```
-v, –verbose 详细模式输出

-q, –quiet 精简输出模式

-c, –checksum 打开校验开关，强制对文件传输进行校验

-a, –archive 归档模式，表示以递归方式传输文件，并保持全部文件属性，等于-rlptgoD

-r, –recursive 对子目录以递归模式处理

-R, –relative 使用相对路径信息

-e, –rsh=COMMAND 指定替代rsh的shell程序

–delete 是指若是Server端删除了一文件，那客户端也相应把这一文件删除，保持真正的一致。

-b, --backup 建立备份，也就是对于目的已经存在有一样的文件名时，将老的文件从新命名为~filename。可使用--suffix选项来指定不一样的备份文件前缀。
--backup-dir 将备份文件(如~filename)存放在在目录下。
-suffix=SUFFIX 定义备份文件前缀
-u, --update 仅仅进行更新，也就是跳过全部已经存在于DST，而且文件时间晚于要备份的文件。(不覆盖更新的文件)
-l, --links 保留软链结
-L, --copy-links 想对待常规文件同样处理软链结
--copy-unsafe-links 仅仅拷贝指向SRC路径目录树之外的链结
--safe-links 忽略指向SRC路径目录树之外的链结
-H, --hard-links 保留硬链结
-p, --perms 保持文件权限
-o, --owner 保持文件属主信息
-g, --group 保持文件属组信息
-D, --devices 保持设备文件信息
-t, --times 保持文件时间信息
-S, --sparse 对稀疏文件进行特殊处理以节省DST的空间
-n, --dry-run现实哪些文件将被传输
-W, --whole-file 拷贝文件，不进行增量检测
-x, --one-file-system 不要跨越文件系统边界
-B, --block-size=SIZE 检验算法使用的块尺寸，默认是700字节
-e, --rsh=COMMAND 指定替代rsh的shell程序
--rsync-path=PATH 指定远程服务器上的rsync命令所在路径信息
-C, --cvs-exclude 使用和CVS同样的方法自动忽略文件，用来排除那些不但愿传输的文件
--existing 仅仅更新那些已经存在于DST的文件，而不备份那些新建立的文件
--delete 删除那些DST中SRC没有的文件
--delete-excluded 一样删除接收端那些被该选项指定排除的文件
--delete-after 传输结束之后再删除
--ignore-errors 及时出现IO错误也进行删除
--max-delete=NUM 最多删除NUM个文件
--partial 保留那些因故没有彻底传输的文件，以是加快随后的再次传输
--force 强制删除目录，即便不为空
--numeric-ids 不将数字的用户和组ID匹配为用户名和组名
--timeout=TIME IP超时时间，单位为秒
-I, --ignore-times 不跳过那些有一样的时间和长度的文件
--size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间
--modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为0
-T --temp-dir=DIR 在DIR中建立临时文件
--compare-dest=DIR 一样比较DIR中的文件来决定是否须要备份
-P 等同于 --partial
--progress 显示备份过程
-z, --compress 对备份的文件在传输时进行压缩处理
--exclude=PATTERN 指定排除不须要传输的文件模式
--include=PATTERN 指定不排除而须要传输的文件模式
--exclude-from=FILE 排除FILE中指定模式的文件
--include-from=FILE 不排除FILE指定模式匹配的文件
--version 打印版本信息
--address 绑定到特定的地址
--config=FILE 指定其余的配置文件，不使用默认的rsyncd.conf文件
--port=PORT 指定其余的rsync服务端口
--blocking-io 对远程shell使用阻塞IO
-stats 给出某些文件的传输状态
--progress 在传输时现实传输过程
--log-format=formAT 指定日志文件格式
--password-file=FILE 从FILE中获得密码
--bwlimit=KBPS 限制I/O带宽，KBytes per second
-h, --help 显示帮助信息
```

### 以上是Windows与Windows之间的同步，那么若是要想和Linux之间进行数据同步或备份呢？

------

### 一、查看selinux机制，关闭selinux

```
[root@node1 ——]# getenforce

Disabled
```

### 二、安装Rsync客户端软件

```
[root@node1 ——]# yum install rsync xinetd
```

### 三、须要安装这两个软件包就能够了，安装好以后就要修改一点配置文件了：

```
[root@node1 ——]# vi /etc/xinetd.d/rsync #编辑配置文件，设置开机启动rsync ,Centos上的rsync使用xinetd启用的

将disable=yes，改成no

service rsync

{

disable = no

socket_type     = stream

wait            = no

user            = root

server          = /usr/bin/rsync

server_args     = --daemon

log_on_failure += USERID

}

/etc/init.d/xinetd start #启动xinetd这个服务
```

### 四、修改以后就远程链接测试一下，记得把windows的防火墙给关闭了哦，要否则极可能会链接不上的，或都在windows防火墙上开放873这个端口

------

![cwrsyncserver4](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/559_fe5_f6d.JPEG)

------

### OK，说明链接到windows上成功了，那接下来咱们就建立一个与windows下同步的目录了：

```
[root@node1 ——]# mkdir pv /rsyncdata/data
```

### 为了同步数据时不用每次都不手动输入密码，咱们在客户端（linux下）也建立一个和服务端（windows）同样的密码文件（此文件路径和密码要与服务器端的同样，客户端不用写名字）

```
[root@node1 ——]# vim /etc/rsyncd.secrets #只须要写上服务器端上的用户密码就能够了
```

![cwrsyncserver](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/560_713_065.png)

------

```
[root@node1 ——]# chmod 600 /etc/rsyncd.secrets  #改一下权限
```

### 五、到这里咱们就能够写命令来拉取windows服务器端上的数据了：

```
[root@node1 ——]# rsync -vazrtopqg --delete --password-file=/etc/rsyncd.secrets rsync@10.17.1.88::rsyncdata/* /rsyncdata/data/
```

![cwrsyncserver4](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/561_221_9cb.JPEG)

------

### 传输成功，这里说明一下这几个参数表示什么：

```
-v, --verbose#详细模式输出

-q, --quiet#精简输出模式

-a, --archive#归档模式，表示以递归方式传输文件，并保持全部文件属性，等于-rlptgoD

-r, --recursive#对子目录以递归模式处理

-o, --owner#保持文件属主信息

-g, --group#保持文件属组信息

-t, --times#保持文件时间信息

--delete#删除那些DST中SRC没有的文件

--password-file=FILE#从FILE中获得密码
```

### 六、为了一是每次有数据修改时都要手动去同步一步，咱们把这个命令写成一个脚本，再添加一个任务计划，这个就能够实现自动同步数据了；

### ![cwrsyncserver](/img-post/开发/操作系统/Windows/CwRsync  Windows与Windows之间同步备份配置详解.assets/376_d9f_66d.JPEG)

------

```
[root@node1 ——]# crontab -e

* * * * * /bin/bash /root/rsync.sh &> /dev/null  #咱们设置每分钟同步一次
```

http://www.javashuo.com/article/p-dnfztdbi-m.html