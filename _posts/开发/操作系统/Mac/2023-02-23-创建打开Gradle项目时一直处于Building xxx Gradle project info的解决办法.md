---
tags:
    - 操作系统
    - Mac
---

创建打开Gradle项目时一直处于Building xxx Gradle project info的解决办法



![](http://images2015.cnblogs.com/blog/571457/201602/571457-20160223154026271-396459729.png)

一直停留在此界面的原因是：Android studio 在下载 Gradle ,但是由网络原因, Gradle 下载不了，所以无法打开。

解决方案：

- 第一种方法：一直等待，我等了差不多半个小时自己下载好了。

- 第二种方法：手动下载

1. 打开：“C:\Users\用户名\.gradle\wrapper\dists”，会看到这个目录下有个 gradle-x.xx-all 的文件夹。这就是我们要手动下载的gradle版本，如果 x.xx 是2.4 ，那我们就要手动下载 2.4 版本。下载地址是 http://gradle.org/gradle-download/

1. 下载完相应版本的gradle之后，将下载的.zip文件（不需要解压）复制到上述的gradle-x.xx-all\6r4uqcc6ovnq6ac6s0txzcpc0 文件夹下。

1. 重启Android Studio(首次启动需要几分钟)

http://www.cnblogs.com/wz122889488/p/5210214.html



最终解决方案：使用本地已经安装好的gradle

![](../../../_resources/33c4b7443c574e6092be9dd37363eef3.png)





