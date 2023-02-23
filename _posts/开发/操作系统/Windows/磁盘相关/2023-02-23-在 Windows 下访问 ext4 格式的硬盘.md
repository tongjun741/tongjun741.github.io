---
tags:
    - 操作系统
    - Windows
    - 磁盘相关
---

## 方案一：ext2explore 软件

这是 windows 下的一个软件，你下载完毕之后，需要右键管理员权限使用，就能打开 ext4 格式硬盘的

下载地址：[http://sourceforge.net/project](https://link.zhihu.com/?target=http%3A//sourceforge.net/projects/ext2read/files/Ext2Read%20ver%202.0/ext2explore%202.0%20beta/)

![img](/img-post/开发/操作系统/Windows/磁盘相关/在 Windows 下访问 ext4 格式的硬盘.assets/v2-8af5ea831aae9ed9c7ec35efda10647b_1440w.jpg)

但这个软件在 Windows 10 安装最新版可能会遇到，不仅把ext4识别成ext3，挂载之后还无法访问，说已损坏，让我格式化的问题。

如果遇到这种问答，推荐大家下载 0.68 的版本，应该是能使用的。

0.68 的下载地址：[Ext2 File System Driver for Windows](https://link.zhihu.com/?target=https%3A//sourceforge.net/projects/ext2fsd/)

## 方案二：Paragon ExtFS

这个软件也可以直接在文件资源管理器里访问 Ext4 硬盘，虽然是付费软件，但刚开始的 10 天是可以免费使用，之后就限速 5MB/s。

下载地址：[ExtFS for Windows®](https://link.zhihu.com/?target=https%3A//china.paragon-software.com/home-windows/extfs-for-windows/download.html)

![img](/img-post/开发/操作系统/Windows/磁盘相关/在 Windows 下访问 ext4 格式的硬盘.assets/v2-3f5a689d8572db44407fc5337a3f824c_1440w.jpg)

## 方案三：DiskInternals Linux Reader

如果上面两个方案还是无法解决，那你就下载这个，下面是软件的官方地址，大家只要下载免费版的就能读取 ext4 了。

[Access to Ext 2/3/4, HFS and ReiserFS from Windows| DiskInternals](https://link.zhihu.com/?target=https%3A//www.diskinternals.com/linux-reader/)

![img](/img-post/开发/操作系统/Windows/磁盘相关/在 Windows 下访问 ext4 格式的硬盘.assets/v2-694a6afeed0b4913b198515ec59c672d_1440w.jpg)



https://zhuanlan.zhihu.com/p/448535639