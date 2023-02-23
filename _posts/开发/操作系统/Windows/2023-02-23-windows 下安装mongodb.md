---
tags:
    - 操作系统
    - Windows
---

windows 下安装mongodb

下载地址:http://www.7edown.com/soft/down/soft_50393.html

好像下载的是32位，系统是64位，所以安装的时候需要加 --journal,然后要手动创建data目录和logs目录，安装成服务时需要指定data目录和log文件路径：



D:\dev\mongodb-win32-i386-3.0.6\bin>mongod  --journal -dbpath "D:\dev\mongodb-win32-i386-3.0.6\data" --install --logpath="D:\dev\mongodb-win32-i386-3.0.6\logs\mongodb.log"





http://www.cnblogs.com/cxd4321/archive/2012/05/18/2507777.html、

http://www.cnblogs.com/flyoung2008/archive/2012/07/18/2597269.html

