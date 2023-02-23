---
tags:
    - 其它
    - Jenkins
---

一、系统管理->节点管理

点击新建节点

![img](https://img2018.cnblogs.com/i-beta/1327931/201911/1327931-20191113105117477-1957237788.png)

 

 输入节点名称，选择固定节点

![img](https://img2018.cnblogs.com/i-beta/1327931/201911/1327931-20191113105208268-957884263.png)

点击确定

注意下拉选择的位置

 

![img](https://img2018.cnblogs.com/i-beta/1327931/201911/1327931-20191113105243121-709987318.png)

 

 上面的启动方式只有2项，这两项其实配置起来很困难。接下来，我们需要改动一个地方，让出来第三项。

 3.到系统管理->全局安全配置

 

![img](https://img2018.cnblogs.com/i-beta/1327931/201911/1327931-20191113110747020-683657862.png)

 

 

保存后，然后返回到新建节点

![img](https://img2018.cnblogs.com/i-beta/1327931/201911/1327931-20191113111128950-1849373132.png)

 

这里特别提醒一点，这个labels名称，以后会用到

 

保存后

![img](https://img2018.cnblogs.com/i-beta/1327931/201911/1327931-20191113111241842-1102541992.png)

 

 

.launch按钮，下载jenkins agent jar到windows机器上

![img](https://img2018.cnblogs.com/i-beta/1327931/201911/1327931-20191113111348586-1509251808.png)

 

 

上面提供两种方法，

第一种下载文件，默认java去启动和运行程序。

第二个，你可以拷贝这个命令，放到一个记事本文件，然后保存为bat文件，双击bat文件也可以运行。当然直接拷贝命令到cmd也可以，bat文件是方便下次你开机之后启动，连接到master环境。



## 退出后自动重启

```
:startjc
   java -jar agent.jar -jnlpUrl https://build.xxx.cn/computer/windows/jenkins-agent.jnlp -secret xxx -workDir "c:\jenkins"
   rem 用ping命令来实现延时运行
   for /l %%i in (1,1,10) do ping -n 1 -w 1000 168.20.0.1>nul
   goto startjc
echo on
```





https://www.cnblogs.com/xiaomifeng0510/p/11848834.html

https://www.cnblogs.com/liuys635/p/11260158.html

