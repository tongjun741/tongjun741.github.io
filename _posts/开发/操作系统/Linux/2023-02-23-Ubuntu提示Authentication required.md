---
tags:
    - 操作系统
    - Linux
---



腾讯云中重置密码后打开部分软件提示：

Authentication required

the password you use to log in to your computer no longer matches that of your keyring



解决方法：

在shell中执行

```
rm ~/.local/share/keyrings/login.keyring
```

后重启



https://blog.csdn.net/songwzup/article/details/51766322