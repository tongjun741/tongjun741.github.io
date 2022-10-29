---
tags:
    - IDE
---

IDEA的配置如何在Windows和Mac之间转换

IDEA中大部分的个性化配置基本都能通过几个步骤完成同步，且可以是不同操作系之间。本文Windows和Mac为例子。（我之前尝试是否可以把配置挂载到FTP上，被多台机器共享，但是考虑到因为网络就无法启动IDEA代价还是相当大，所以暂时没去研究 ~)



Windows系统IDEA安装后，其主要的个性化配置都会默认在:



C:\Users\Administrator\.IntelliJIdea2017.3\config

1





config 目录是 IntelliJ IDEA 个性化化配置目录，或者说是整个 IDE

设置目录。是最重要的目录。这个目录主要记录了：IDE 主要配置功能、自定义的代码模板、自定义的文件模板、自定义的快捷键、Project

的tasks 记录等等个性化的设置 ；

system 目录是 IntelliJ IDEA 系统文件目录，是 IntelliJ IDEA

与开发项目一个桥梁目录，里面主要有：缓存、索引、容器文件输出等等，也是最不可或缺目录之一；

此时，我要将IDEA的个性化配置安装到另一台Mac系统的电脑：

1、首先，借助IDEA导出所有配置，得到settings.jar的配置文件；





2、将window系统中上述的config文件夹拷贝到Mac系统的任意文件路径下；



3、在Mac系统安装好IDEA后，首先导入步骤1的settings.jar;





4、最后进入IDEA的包路径下找到文件：

/Applications/IntelliJ IDEA.app/Contents/bin/idea.properties

此文件为IDEA的配置文件，可以配置一些IDEA的启动属性。我们在此配置文件中指定IDEA的个性化配置路径（idea.config.path）和一些其他的配置，其中idea.config.path必须指定到步骤二所拷贝的config文件夹；



idea.properties：（其中插件和日志的位置需要自己指定）

idea.config.path=/Users/qudian/Documents/IntelliJIDEA/config

idea.system.path=/Users/qudian/Documents/IntelliJIDEA/system



idea.plugins.path=.......

idea.log.path=......

1

2

3

4

5

5、最后启动IDEA，大工告成，换什么系统都不需要为IDEA配置耗费时间；（最后记得把设置中的keymap更改为 MAC OS X）

————————————————

版权声明：本文为CSDN博主「WangCw的夏天」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/qq_33404395/java/article/details/82910272

