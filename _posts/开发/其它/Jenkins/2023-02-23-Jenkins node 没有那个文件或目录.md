---
tags:
    - 其它
    - Jenkins
---

Jenkins node 没有那个文件或目录

把要用的命令创建一个快捷方式到/usr/bin，如 ln -s /usr/local/bin/node /usr/bin/，这样在Jenkins shell中就能用到node命令了





http://www.jianshu.com/p/d9191eed6950



Jenkins上踩过的那些坑

![144](http://upload.jianshu.io/users/upload_avatars/436630/560ccb5c5e40.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/144/h/144)

 

作者 吴彦欣 关注

2015.12.14 23:38* 字数 602 阅读 3291评论 11喜欢 2

![](http://upload-images.jianshu.io/upload_images/436630-980297d7de13473d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



在学习搭建Jenkins CI环境时踩过许多大坑小坑，记录了一些下来，以作备忘

有些坑踩完就忘了，只记录下记得的

后续研究时候继续踩坑再更新

1. Failed to connect to repository

为job设置git repository的时候报Failed to connect to repository

解决办法：本地安装git客户端，或者在Jenkins全局系统设置那里指定git的执行路径。

2. No JDK named ?null? found

任务执行失败的console里面有这样一句话：No JDK named ?null? found

解决办法：在Jenkins系统设置中指定有效JAVA_HOME路径

3. node: command not found

针对command not found 我主要总结两种解决方案，对于网上说的那些方法我没看懂

- 方案一：如果你使用service jenkins start启动了jenkins进程，那么久有可能出现Jenkins运行环境跟用户不同。最简单粗暴的方法是使用 java -jar /usr/lib/jenkins/kenkins.war

- 方案二：把要用的命令创建一个快捷方式到/usr/bin，如 ln -s /usr/local/bin/node /usr/bin/，这样在Jenkins shell中就能用到node命令了。当然如果是node命令找不到的话可以直接使用Nodejs Plugin解决

4. 如何设置源码库浏览器

在Jenkins使用Git SCM的时候有一项源码库浏览器的设置，起初不知道有何用，只是看了说明大概知道是会对每次build生成changes，然后并没有告诉怎么设置，选择一种浏览器后要填一个URL，然后就各种百度谷歌没找到答案。最后自己随便填了一个，build了一下，点击changes里面的链接，报404，看了一下URL的生成规则，才知道怎么设置。

对于githubweb的URL应该填https://github.com/your_name/your_repo_name/

5. 单独发送邮件给对构建造成不良影响的责任人

![](http://upload-images.jianshu.io/upload_images/436630-af8c3092af3081ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





勾选了这个选项之后没什么反应，如果用的是git源码库，可以在系统设置中勾选自动创建提交人

6. 设置完权限之后无法登录怎么办

sudo vi /var/lib/jenkins/config.xml

将<useSecurity>true</useSecurity>改为false

重启Jenkins，重新设置权限

7. 为Jenkins配ssh

可以直接将你的密钥放到/var/lib/jenkins/.ssh/下

也可以使用ssh的插件完成

