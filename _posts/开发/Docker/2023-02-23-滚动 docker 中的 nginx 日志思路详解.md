---
tags:
    - Docker
---

Nginx 自己没有处理日志的滚动问题，它把这个球踢给了使用者。一般情况下，你可以使用 logrotate 工具来完成这个任务，或者如果你愿意，你可以写各式各样的脚本完成同样的任务。本文笔者介绍如何滚动运行在 docker 中的 nginx 日志文件



**思路**

Nginx 官方其实给出了如何滚动日志的说明：

Rotating Log-files
In order to rotate log files, they need to be renamed first. After that USR1 signal should be sent to the master process. The master process will then re-open all currently open log files and assign them an unprivileged user under which the worker processes are running, as an owner. After successful re-opening, the master process closes all open files and sends the message to worker process to ask them to re-open files. Worker processes also open new files and close old files right away. As a result, old files are almost immediately available for post processing, such as compression.

这段说明的大意是：

•先把旧的日志文件重命名
•然后给 nginx master 进程发送 USR1 信号
•nginx master 进程收到信号后会做一些处理，然后要求工作者进程重新打开日志文件
•工作者进程打开新的日志文件并关闭旧的日志文件

其实真正需要我们做的工作只有前面两点！

创建测试环境

假设你的系统中已经安装好了 docker，这里我们直接运行一个 nginx 容器：

```
$ docker run -d \`` ``-p 80:80 \`` ``-``v` `$(``pwd``)``/logs/nginx``:``/var/log/nginx` `\`` ``--restart=always \`` ``--name=mynginx \`` ``nginx:1.11.3
```

注意，我们把 nginx 的日志绑定挂载到了当前目录下的 logs 目录下。

把下面的内容保存到 test.sh 文件中：

```
#!/bin/bash``for` `((i=1;i<=100000;i++))``do`` ``curl http:``//localhost` `> ``/dev/null`` ``sleep` `1``done
```

然后运行这个脚本，就可以模拟产生连续的日志记录。

创建滚动日志的脚本

创建 rotatelog.sh 文件，其内容如下：

```
#!/bin/bash
getdatestring()
{
 TZ='Asia/Chongqing' date "+%Y-%m-%d"
}
datestring=$(getdatestring)
mv /var/log/nginx/access.log /var/log/nginx/access.${datestring}.log
mv /var/log/nginx/error.log /var/log/nginx/error.${datestring}.log
gzip /var/log/nginx/error.${datestring}.log
kill -USR1 `cat /var/run/nginx.pid`
```



getdatestring 函数取当前的时间并格式化为字符串，比如 "201807241310"，笔者比较喜欢用日期和时间来命名文件。注意这里通过 TZ='Asia/Chongqing' 指定了时区，因为默认情况下格式化的是 UTC 时间，用起来怪怪的(要实时脑补 +8 小时)。下面的两条 mv 命令用来重命名日志文件。最后通过 kill 命令向 nginx master 进程发送 USR1 信号。

通过下面的命令为 rotatelog.sh 文件添加可执行权限并复制到 $(pwd)/logs/nginx 目录下：

```
$ ``chmod` `+x rotatelog.sh``$ ``sudo` `cp` `rotatelog.sh $(``pwd``)``/logs/nginx
```

**定时执行滚动操作**

我们的 nginx 运行在容器中，所以需要在容器中给 nginx master 进程发送 USR1 信号。因此我们需要通过 docker exec 命令在 mynginx 容器中执行 rotatelog.sh 脚本：

```
$ docker exec mynginx bash /var/log/nginx/rotatelog.sh
```

执行一次上面的命令，会如期产生一批新的日志文件：

![img](/img-post/开发/Docker/滚动 docker 中的 nginx 日志思路详解.assets/201808300831422.png)

下面我们把这个命令配置在定时任务中，让它每天早上 1 点钟执行一次。执行 crontab -e 命令，并在文件的末尾添加下面的行：

```
* 1 * * * docker exec mynginx bash /var/log/nginx/rotatelog.sh
```

![img](/img-post/开发/Docker/滚动 docker 中的 nginx 日志思路详解.assets/201808300831422.png)

保存并退出就可以了。下图是笔者测试过程中每 5 分钟滚动一次的效果：

![img](/img-post/开发/Docker/滚动 docker 中的 nginx 日志思路详解.assets/201808300831424.jpg)

为什么不在宿主机中直接 mv 日志文件？

理论上这么做是可以的，因为通过绑定挂载的数据卷中的内容从宿主机上看和从容器中看都是一样的。但是真正这么做的时候你很可能碰到权限问题。在宿主机中，你一般使用的是普通用户，而在容器中产生的日志文件的所有者是会是特殊的用户，并且一般不会给其它用户写和执行的权限：


![img](/img-post/开发/Docker/滚动 docker 中的 nginx 日志思路详解.assets/201808300831425.png)

当然，如果你在宿主机中使用的是 root 用户就不会有问题。

能从宿主机中发送的信号吗？

其实这个问题的全称应该是：能从宿主机中给 docker 容器中的 nginx master 进程发送信号吗？

答案是，可以的。

笔者这[《在 docker 容器中捕获信号》](https://www.jb51.net/article/124687.htm)一文中介绍了容器中信号的捕获问题，感兴趣的朋友可以去看看。在那篇文章中我们介绍了 docker 向容器中进程发送信号的 kill 命令。我们可以通过命令：

```
$ docker container kill mynginx -s USR
```

向容器中的 1 号进程(nginx master)发送 USR1 信号(这种方式只能向 1 号进程发送信号)：

![img](/img-post/开发/Docker/滚动 docker 中的 nginx 日志思路详解.assets/201808300831426.png)

结合上面的两个问题，我们可以写出另外的一种方式来滚动 docker 中的 nginx 日志。这种方式不需要通过 docker exec 命令在容器中执行命令，而完全在宿主机中完成所有的操作：

•先重命名容器数据卷中的日志文件
•给容器中的 1 号进程发送 USR1 信号

参考：

[How To Configure Logging and Log Rotation in Nginx on an Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-configure-logging-and-log-rotation-in-nginx-on-an-ubuntu-vps)

[How To Manage Logfiles with Logrotate on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-manage-logfiles-with-logrotate-on-ubuntu-16-04)



https://www.jb51.net/article/146513.htm