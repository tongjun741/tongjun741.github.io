---
tags:
    - 其它
    - Jira及Confluence
---

Confluence连接Jira用户目录的配置页面在：

![image-20210823105540423](/img-post/开发/其它/Jira及Confluence/Confluence连接Jira的问题.assets/image-20210823105540423.png)

如果Jira用户目录在第一位是不可编辑的，需要把顺序调整到下一位：

![image-20210823105613660](/img-post/开发/其它/Jira及Confluence/Confluence连接Jira的问题.assets/image-20210823105613660.png)



测试报错：

```
com.atlassian.crowd.exception.ApplicationPermissionException: Forbidden (403)

```

确认Jira中的用户服务器IP地址是否包含了Confluence的ip，如果是通过nginx转发的，需要填写nginx的ip：

![image-20210823105311893](/img-post/开发/其它/Jira及Confluence/Confluence连接Jira的问题.assets/image-20210823105311893.png)



对应的Jira配置页面在：

![image-20210823105821499](/img-post/开发/其它/Jira及Confluence/Confluence连接Jira的问题.assets/image-20210823105821499.png)
