---
tags:
    - 操作系统
    - Windows
---

WINDOWS下更改MYSQL数据路径(datadir)

1：查看MySQL数据库data目录位置：

用show variables like 'datadir'，可查看真正的data目录

 

2：修改my.ini数据存放路径：

basedir="D:/Program Files/MySQL/MySQL Server 5.5/" 

datadir="D:/Program Files/MySQL/MySQL Server 5.5/Data/"

 

3：拷贝原Data目录中的所有文件到新的Data目录中

4：重新启动mysql服务 



http://blog.csdn.net/dmz1981/article/details/8509477

