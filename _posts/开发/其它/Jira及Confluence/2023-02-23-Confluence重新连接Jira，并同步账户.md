---
tags:
    - 其它
    - Jira及Confluence
---

最近更新了系统，后来发现Confluence连不上Jira了，导致jira用户登录不了，现在记录一下解决流程。

# 1. 开启Confluence故障恢复账号

在文件**Install_path/bin/setenv.sh**中，添加启动参数如下：



```objectivec
# 123456为密码，可自行替换
CATALINA_OPTS="-Datlassian.recovery.password=123456"
```

![image-20210414152257055](/img-post/开发/其它/Jira及Confluence/Confluence重新连接Jira，并同步账户.assets/image-20210414152257055.png)



然后重启Confluence，用恢复账号：”**recovery_admin**“

![image-20210414152309105](/img-post/开发/其它/Jira及Confluence/Confluence重新连接Jira，并同步账户.assets/image-20210414152309105.png)



# 2. 配置Crowd服务器

![image-20210414152326634](/img-post/开发/其它/Jira及Confluence/Confluence重新连接Jira，并同步账户.assets/image-20210414152326634.png)

Confluence配置

![image-20210414152341364](/img-post/开发/其它/Jira及Confluence/Confluence重新连接Jira，并同步账户.assets/image-20210414152341364.png)

Jira配置

![image-20210414152358561](/img-post/开发/其它/Jira及Confluence/Confluence重新连接Jira，并同步账户.assets/image-20210414152358561.png)

# 3. 重建应用程序连接（按需可选）

![image-20210414152427797](/img-post/开发/其它/Jira及Confluence/Confluence重新连接Jira，并同步账户.assets/image-20210414152427797.png)

![image-20210414152439615](/img-post/开发/其它/Jira及Confluence/Confluence重新连接Jira，并同步账户.assets/image-20210414152439615.png)



作者：咦咦咦萨
链接：https://www.jianshu.com/p/0191c673217f
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。