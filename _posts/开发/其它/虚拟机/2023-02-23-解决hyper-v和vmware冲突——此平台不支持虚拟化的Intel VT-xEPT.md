---
tags:
    - 其它
    - 虚拟机
---

我这里是专业版的

具体操作：

1. 打开“启用关闭windows功能”，取消勾选“Hyper-V”和“Windows虚拟机监控程序平台”
2. 在“服务”中禁用所有与Hyper-V的服务
3. 打开Windows PowerShell（管理员），运行命令：bcdedit /set hypervisorlaunchtype off
4. 运行“msinfo32”，右侧“基于虚拟化的安全性”如果不是未启用，就运行“gpedit.msc”，计算机配置>管理模板>系统>Device Guard>打开虚拟化安全性，“未配置”改成“禁用”，确定后关闭组策略

建议：每操作一步重启一次电脑



https://tmd.pet/post/15.html