---
tags:
    - 操作系统
    - Windows
---

使用命令查看当前版本：

```
DISM /online /Get-CurrentEdition
```

![img](/img-post/开发/操作系统/Windows/WINDOWS SERVER 2019 SERVERDATACENTEREVAL数据中心评估版升级成 正式版.assets/image-1.png)可以看到当前版本为serverDatacenter Eval 评估版本

使用DISM命令升级

```
DISM /online /Set-Edition:ServerDatacenter /ProductKey:WMDGN-G9PQG-XVVXX-R3X43-63DFG /AcceptEula 
```

![img](/img-post/开发/操作系统/Windows/WINDOWS SERVER 2019 SERVERDATACENTEREVAL数据中心评估版升级成 正式版.assets/image-2.png)升级到100%后选择Y重新气动服务器

注：set-edition: 版本信息 key为密钥信息

这里的命令是升级windows server 2019 数据中心版的例子，如果是其他版本，需要替换掉对应的key和Edition，如果密钥不行可以自行搜索尝试

附一个KMS激活:

```
slmgr /skms kms.shuax.com
slmgr /ato
```

http://www.xitongcheng.com/jiaocheng/dnrj_article_47179.html

**winserver2019 kms密钥激活步骤：**

1、右键开始图标，选择Windows PowerShell(管理员)，或者命令提示符(管理员)；

![img](/img-post/开发/操作系统/Windows/WINDOWS SERVER 2019 SERVERDATACENTEREVAL数据中心评估版升级成 正式版.assets/image-3.png)

2、依次执行下面的命令，分别表示安装windows server 2019密钥，设置kms服务器，激活windows server 2019系统，查询激活状态，kms激活一般180天，到期后再次激活。[可用的kms激活服务器有哪些](http://www.xitongcheng.com/jiaocheng/dnrj_article_44606.html)。

```
slmgr /ipk WMDGN-G9PQG-XVVXX-R3X43-63DFG
slmgr /skms zh.us.to
slmgr /ato 
slmgr /xpr  
```



http://blog.vapeun.com/?p=330