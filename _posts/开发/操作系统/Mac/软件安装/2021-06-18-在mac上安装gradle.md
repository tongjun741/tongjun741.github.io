---
tags:
    - 操作系统
    - Mac
    - 软件安装
---

在mac上安装gradle

在mac上安装gradle（超详细，直接按步骤操作即可轻松搞定）




第一步， 就是先download最新版本的gradle，网址如下：


http://gradle.org/gradle-download/


然后将下载下来的zip包放解压到本地任意的路径上，


例如，我本地则安装在


/Users/lixiang/gradle-2.7 


1、打开Mac上的“终端”，输入以下命令,将gradle的bin目录添加至到环境变量中:





vi ~/.bash_profile





2、打开.bash_profile 文件窗口依次输入以下命令：


i （进入vi的编辑模式，添加如下配置）


GRADLE_HOME=/Users/lixiang/gradle-2.7


export GRADLE_HOME


export PATH=$PATH:$GRADLE_HOME/bin


3、输入完毕，按esc键退出vi的编辑模式，


输入“:”(冒号)进入最后行模式，


输入 wq 保存并退出vi


4、通过以下命令来查看是否安装成功：


gradle -version  


如果输出以下的gradle版本信息就表示已经安装成功了：


------------------------------------------------------------




Gradle 2.7




------------------------------------------------------------




Build time:   2015-09-14 07:26:16 UTC




Build number: none




Revision:     c41505168da69fb0650f4e31c9e01b50ffc97893




Groovy:       2.3.10




Ant:          Apache Ant(TM) version 1.9.3 compiled on December 23 2013




JVM:          1.8.0_40 (Oracle Corporation 25.40-b25)




OS:           Mac OS X 10.10.4 x86_64








注意: 如果提示没有gradle命令，则可以检查：





1. GRADLE_HOME路径在环境变量中配置是否正确，


2.确定是否指定到bin目录；








http://blog.csdn.net/u014749862/article/details/48982925



https://gradle.org/releases

