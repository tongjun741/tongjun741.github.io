---
tags:
    - 操作系统
    - Linux
    - 安装系统
---

开机后进入试用界面，打开终端。

```
# 检查当前使用的磁盘
fdisk -l

# 开始分区
cfdisk /dev/sda
# 选择"dos"(如果是uefi就选择"gpt",MBR选择"dos")
```

然后双击桌面的Install开始安装系统。
到分区时选择手动分区，建议这样分配(针对MBR方式)

分区        挂载点	    文件系统	    大小
启动分区    /boot	    ext2    	    200M
EFI分区                                 200M
主分区	    /	        ext4	        自己决定
交换分区	[swap]	    [swap]      	自己决定


后面就没区别了。

安装好系统后可以通过以下命令确认：
```
sudo parted -l
# 如果是MBR会输出msdos
```


https://www.programminghunter.com/article/1298253376/
https://www.muzijie.com/a1/dez54xwe.html