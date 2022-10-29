---
tags:
    - 操作系统
    - Windows
---

从 Win10 1809 和 Windows Server 2019 开始 Windows 开始支持 OpenSSH Server。本文介绍一下其基本的概念和配置方法，本文演示用的环境为 Win10 1809(ssh 客户端)和 Windows Server 2019(ssh 服务器)。

# 安装 OpenSSH Server

OpenSSH 客户端程序默认已经被系统安装好了，打开 Settings->Apps->Manage optional features 面板就可以看到：

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 开启ssh.assets/952033-20181223221242458-833263915.png)

而 OpenSSH Server 默认没有安装，需要用户手动安装。点击上图中的 "Add a feature" 按钮，然后选择 OpenSSH Server，并点击 "Install" 按钮：

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 开启ssh.assets/952033-20181223221323946-125463532.png)

**开启服务**
安装完成后打开服务管理器，把 OpenSSH Authentication Agent 服务和 OpenSSH SSH Server 服务都设置为自启动，并启动这两个服务：

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 开启ssh.assets/952033-20181223221416682-1445089775.png)

**监听端口**
启动服务后可以通过 netstat 命令查看 SSH Server 服务是不是已经开始监听默认的 22 号端口了：

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 开启ssh.assets/952033-20181223221456333-1862174148.png)

**防火墙规则**
在安装 OpenSSH Server 的时候会在防火墙的入站规则中添加一条记录让防火墙放行对 22 号端口的访问：

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 开启ssh.assets/952033-20181223221535450-94522261.png)

添加Jenkins账户，设置为Administrator组，这里可以限制权限，这里简单起见就这么设置

这样就可以远程连接了，但是远程连接成功后默认的 shell 是 Windows Command shell (cmd.exe) 程序，在powershell 大行其道的时候应该修改为powershell 为默认程序

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 开启ssh.assets/39184-20210827231658674-1747563280.png)

 

其实就是在**运行 OpenSSH Server 的 Windows 系统的注册表中**添加一个配置项，注册表路径为 HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH，项的名称为 DefaultShell，项的值为 C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe。我们可以以管理员身份启动 PowerShell，然后执行下面的命令完成注册表项的添加：

注意：这个设置推荐作为 本机需要执行很多powershell的机器,比如最终的部署机器. 有一种情况需要注意，这样设置反而不好（如果设置了可以删除这条注册表项目），当该Windows 作为 Slave 机器的时候，作为Master 连接 Slave 的时候很多命令执行不了，因为 jenkins 还是认为是Windows Command shell，写的连接node的程序也是针对 cmd的。

```
> New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
```

#  居然报错了

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 开启ssh.assets/39184-20210827232239147-1229812765.png)

 

 

# Powershell脚本的4种执行权限介绍

- Restricted - 默认的设置， 不允许任何script运行
- AllSigned - 只能运行经过数字证书签名的script
- RemoteSigned - 运行本地的script不需要数字签名，但是运行从网络上下载的script就必须要有数字签名
- Unrestricted - 允许所有的script运行

> windows默认不允许任何脚本运行，可以使用 "Set-ExecutionPolicy" cmdlet 来更改这个策略。

修改方法：

右键 powershell 图标 选择 “Run as Administrator”， 执行以下语句 -> "y" 即可。

```
Set-ExecutionPolicy Unrestricted
```

![img](/img-post/开发/操作系统/Windows/Windows Server 2019 开启ssh.assets/39184-20210827232507062-2104140702.png)

https://www.cnblogs.com/deepinnet/p/15195660.html

https://blog.csdn.net/lh1162810317/article/details/94479899 