---
tags:
    - NodeJS
    - 自动测试
---





# ChromeDriver installation failed { Error: EACCES: permission denied, mkdir

# 问题描述

npm在安装chromedriver的时候报错
安装命令

```
npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver
1
```

报错描述
![在这里插入图片描述](/img-post/开发/NodeJS/自动测试/ChromeDriver installation failed.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1OTgzNTc5,size_16,color_FFFFFF,t_70.png)

# 问题分析

缺少权限降级导致的

# 解决方式

添加–unsafe-perm 参数，如

```
npm install  --unsafe-perm
1
```

或者安装淘宝镜像

```
npm install --registry=https://registry.npm.taobao.org  --unsafe-perm
1
```

一劳永逸的方法：
（针对当前用户的）

```
npm config set unsafe-perm
1
```

(全局的）

```
npm config -g set unsafe-perm
```



https://blog.csdn.net/qq_25983579/article/details/103035302