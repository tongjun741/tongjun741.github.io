---
tags:
    - 其它
    - Git
    - GitHub Actions
---

## 结论：macOS下使用VirtualBox启动Windows 7测试虚拟机非常慢

改了各种配置都不行。



微软官方提供的Windows 7测试虚拟机（VirtualBox）：

```
# 下载
wget https://ia601000.us.archive.org/20/items/ie11.win7.virtualbox/IE11.Win7.VirtualBox.zip
# 解压缩
unzip IE11.Win7.VirtualBox.zip
# 导入到VirtualBox
VBoxManage import "IE11 - Win7.ova"
# 设置虚拟机允许远程访问
VBoxManage modifyvm "IE11 - Win7" --vrde on
VBoxManage modifyvm "IE11 - Win7" --vrdeport 3389
# 启动虚拟机
VBoxManage startvm "IE11 - Win7" --type headless
# 查看运行中的虚拟机
VBoxManage  list runningvms
# 对虚拟机截屏
VBoxManage controlvm "IE11 - Win7" screenshotpng a.png
# 向虚拟机发送关闭电源的信号
VBoxManage controlvm "IE11 - Win7" acpipowerbutton
# 强制关闭虚拟机
VBoxManage controlvm "Win7" poweroff
# 删除虚拟机
VBoxManage unregistervm "Win7" --delete
```



安装Virtual Box扩展，并自动同意协议：

```
wget https://download.virtualbox.org/virtualbox/6.1.38/Oracle_VM_VirtualBox_Extension_Pack-6.1.38.vbox-extpack
sudo VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-6.1.38.vbox-extpack  --accept-license=33d7284dc4a0ece381196fda3cfe2ed0e1e8e7ed7f27b9a9ebc4ee22e24bd23c
```



下载链接来源：

https://gist.github.com/zmwangx/e728c56f428bc703c6f6#gistcomment-3115797

参考：

https://medium.com/@akshaymohite/setting-up-vagrant-virtual-box-on-mac-os-359a32257700

https://www.softlay.com/downloads/windows-7?download=links

https://www.softlay.com/downloads/windows-10?download=links

https://github.com/actions/runner-images/issues/183
