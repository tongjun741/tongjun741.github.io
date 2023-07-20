---
tags:
    - NAS
---

> 理论上适用DSM7.X，实测DSM7.1.1-46962 Update4，Active Backup for Business 套件版本2.5.0-12631，其余版本自行测试。

# 套件安装

直接在套件中心安装 Active Backup for Business 套件 
![image-1678331976552](/img-post/开发/NAS/黑群晖Active Backup for Business 套件激活.assets/image-1678331976552.png)
安装完毕后打开ABB套件，查看是否需要激活，如需激活，直接关闭进行下一步。

# 复制序列号SN

激活前先去控制面板中系统信息复制系统SN序列号，后面需要用到。

# 套件激活

在浏览器中输入：

```none
http://群晖地址:5000/webapi/auth.cgi?api=SYNO.API.Auth&version=3&method=login&account=管理员用户名&passwd=密码&format= cookie
```

或者

```none
https://群晖地址:5001/webapi/auth.cgi?api=SYNO.API.Auth&version=3&method=login&account=管理员用户名&passwd=密码&format= cookie
```

查看网页返回内容最后是否带有 **“success”:true** 字样

接着在浏览器中输入：

```none
http://群晖地址:5000/webapi/entry.cgi?api=SYNO.ActiveBackup.Activation&method=set&version=1&activated=true&serial_number="SN序列号"
```

或者

```none
https://群晖地址:5001/webapi/entry.cgi?api=SYNO.ActiveBackup.Activation&method=set&version=1&activated=true&serial_number="SN序列号"
```

正常情况激活成功的话网页返回内容应是：

```none
{"data":{"activated":true},"success":true}
```

至此Active Backup for Business 套件激活就已经完成了

# 补充

- 上面给到的网址优先选用5000端口，实测5001也可以，如果5000不行可以尝试5001
- 理论上不限制浏览器，实测Mac OS 的Safari浏览器不行，最后是Google Chrome实测可以
- 理论上所有操作是在局域网内操作，实测外网DDNS也可以，有公网的小伙伴可以试试
- 如果UI后返回内容为：{“error”:{“code”:119},“success”:false}，可检查链接中替换内容以及尝试更换浏览器



http://naeeo.com/archives/513