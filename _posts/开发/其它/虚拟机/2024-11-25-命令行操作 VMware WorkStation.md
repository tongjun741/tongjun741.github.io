---
tags:
    - 其它
    - 虚拟机
---

Windows 环境下，需要使用命令后台开启虚拟机。
操作环境：Windows 10 工作站版、VMware WorkStation 16 Pro

通过使用 vmrun 命令可以对虚拟机进行后台开机操作。

```
ps
cd C:\Program Files (x86)\VMware\VMware Workstation>

.\vmrun.exe start D:\VMware\Win10\Win10.vmx nogui
```

常见命令及详解：

```
ps
# 启动虚拟机操作
# -T 可指定宿主主机类型，合法的类型包括：ws（vmware workstation）|server|server1|fusion|esx|vc|player，其中ws、esx、player较为常用
# nogui 表示无图形界面启动，也可以指定为gui表示图形界面启动
.\vmrun.exe -T ws start guest.vmx nogui | gui

# 关闭虚拟机操作
# hard 表示硬关机，也可以指定为soft表示软关机
.\vmrun.exe stop guest.vmx hard | soft

# 重启虚拟机操作
# hard 表示硬重启，也可以指定为soft表示软重启
.\vmrun.exe reset guest.vmx hard | soft

# 暂停虚拟机
.\vmrun.exe pause guest.vmx

# 停止暂停虚拟机 
.\vmrun.exe unpause guest.vmx
   
# 列出正在运行的虚拟机
.\vmrun.exe list

# 获取关于vmrun命令行的帮助
.\vmrun.exe -h
```

https://www.txisfine.cn/archives/ca8ec92c