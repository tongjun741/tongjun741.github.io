---
tags:
    - 操作系统
    - Windows
---

# [Windows Server 2019 AD域控搭建](https://www.cnblogs.com/js1314/p/14312065.html)

https://www.cnblogs.com/js1314/p/14312065.html

参考地址：https://cloud.tencent.com/developer/news/468223

 

一、使用AD域的好处

大企业为了统一管理电脑，公司IT人员都会在公司搭建域控，将公司所有电脑都加入到域中，然后进行权限的集中管理。

公司员工电脑加入域后，可以控制员工的登录权限，文件访问权限，打印权限，电脑配置修改权限等。

阻止一些软件自动升级，用户私自安装软件，电脑的安全得到了一定的防范。

对于用户集中权限管理后，IT管理工作效率高，现在很多企业的ERP软件，加密软件都和AD域进行帐户绑定，分配权限。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0.jpeg)

软件安装，可以进行软件的统一下发等等，减少IT管理工作量。

等等。

二、搭建AD域

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715543.jpeg)

1、安装windows server 2019操作系统。

2、选中AD域服务器，修改静态IP地址，将DNS设置为127.0.0.1。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715645.jpeg)

3、修改AD域服务器的主机名称，自己定义。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715634.jpeg)

4、打开服务器管理器，找到右上角“管理”--“添加角色和功能”。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715650.jpeg)

5、“开始之前”，启用向导，进行角色或功能添加，下一步。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715460.jpeg)

6、选择默认的“基于角色或基于功能的安装”，下一步。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715636.jpeg)

7、“从服务器池中选择服务器”，现在只有一台，所以默认就选择NJAD01，下一步。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715506.jpeg)

8、找到“Active Directory域服务”，勾选。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715574.jpeg)

9、勾选后，会跳出如下需要安装的角色或功能，点击“添加功能”。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715624.jpeg)

10、现在Active Directory域服务，已经选中，点击下一步。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715626.jpeg)

11、功能，下一步。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715718.jpeg)

12、ADDS，下一步。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715911.jpeg)

13、选择“安装”。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715756.jpeg)

14、正在安装Active Directory 域服务。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715735.jpeg)

15、看到已在NJAD01上安装成功，说明AD域的角色添加成功了。那是不是AD域现在就配置完成了呢？

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715679.jpeg)

16、AD域服务的角色安装成功后，搭建域控的任务还没有结束，还需要进行部署配置。

选择右上角，带感叹号的旗子，点击“将此服务器升级为域控制器”。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715751.jpeg)

17、部署配置，如果是公司的首台域控制器，选择“添加新林”，自定义公司的根域名，域名命令规则xxx.com，为什么要用.com结尾呢？一般来说.com注册用户为公司或企业。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715782.jpeg)

18、新林和根域的功能级别，默认Windows Server 2016。

指定域控制器功能，域名系统（DNS）服务器，全局编录（GC）都是默认安装在此域控服务器上。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715821.jpeg)

19、设置目录服务器叫还原模式（DSRM）密码。Directory Services Restore Mode，简称DSRM，又称目录服务恢复模式。是Windows域控制器的服务器安全模式启动选项。DSRM允许管理员用来修复或还原修复或重建活动目录数据库。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715859.jpeg)

20、DNS选项，下一步。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715977.jpeg)

21、其它选项，NetBIOS域名，默认，下一步。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716041.jpeg)

22、AD DS数据库、日志文件和SYSVOL的文件存储的默认位置。

Ntds.dit：存储活动目录数据库文件，存储域控制器的所有活动目录对象。

Edb*.log：存储日志文件，默认日志文件Edb.log。

SYSVOL，它是用来存储域公共文件服务器副本的共享文件夹，例如我们用得最多的组策略设置、脚本等都是存在这个共享目录中的，script脚本文件，Netlogon共享文件，Sysvol共享文件。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715867.jpeg)

23、查看选项中，可以看查看到上面的部署全部信息参数。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715845.jpeg)

24、刚才部署了那么多，可以查看刚才的配置脚本。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715886.jpeg)

25、“先决条件检查”。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715947.jpeg)

发现有一个报错，本地Administrator帐户将成为域Administrator帐户。无法新建域，因为本地的域Administrator帐户密码不符合要求。

这个报错，就是说域服务器的本地Administrator帐户要升级成域Administrator帐户了，之前设置的密码太简单了，需要重新设置一下。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716009.jpeg)

打开dos界面，输入命令，命令完成。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173715983.jpeg)

命令完成后，重置本地管理员密码。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716119.jpeg)

26、点击上一步后，然后点下一步，重新检测“先决条件检查”。发现可以正常通过条件检查。点击“安装”。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716088.jpeg)

27、正在安装中...

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716044.jpeg)

28、安装完成后，重新启动域服务器。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716161.jpeg)

29、输入密码，登录域服务器。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716166.jpeg)

30、在升级成为域服务器后，计算机管理中，就没有用户和组了。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716202.jpeg)

31、用户和组都在“工具”--“Active Directory用户和计算机”中。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716100.jpeg)

32、进入“Active Directory用户和计算机”后，找到Users可以查找到Administrator等用户。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716170.jpeg)

33、域服务器已经搭建完成。

 

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 AD域控搭建.assets/0-20210315173716351.jpeg)