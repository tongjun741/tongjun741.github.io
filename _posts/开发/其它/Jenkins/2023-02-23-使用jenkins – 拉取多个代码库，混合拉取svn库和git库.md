---
tags:
    - 其它
    - Jenkins
---

### 问题描述：

在[jenkins](https://www.kktoo.com/tag/jenkins/)配置中，有些项目需要依次拉取多个代码仓库；有些项目还会需要既拉取svn代码库，又拉取git代码库。默认的配置是不支持这样操作的。

### 解决方法：

使用Multiple SCMs插件。



#### 一、安装插件

插件安装完成后，在源码管理单选框中会新增一个选项，如下图所示：

![使用jenkins - 拉取多个代码库，混合拉取svn库和git库](/img-post/开发/其它/Jenkins/使用jenkins – 拉取多个代码库，混合拉取svn库和git库.assets/multiplescms01.png)



#### 二、添加代码库

点击Add SCM打开下拉列表，可以看到多个代码托管平台的选项，选择我们需要的Git或者Subversion(SVN)，如下图所示：

![使用jenkins - 拉取多个代码库，混合拉取svn库和git库](/img-post/开发/其它/Jenkins/使用jenkins – 拉取多个代码库，混合拉取svn库和git库.assets/multiplescms02.png)



#### 三、配置代码库

如果需要拉取多个代码库，就通过Add SCM多次添加，并配置仓库url，分支名称，用户等等。

当然，也可以添加多个Git代码库，或者加多个SVN代码库，或者Git代码库和SVN代码库混合使用。

如下图所示，添加了2个Git代码仓库：

![使用jenkins - 拉取多个代码库，混合拉取svn库和git库](/img-post/开发/其它/Jenkins/使用jenkins – 拉取多个代码库，混合拉取svn库和git库.assets/multiplescms03.png)

https://www.kktoo.com/jenkins-use-multiple-scms.html