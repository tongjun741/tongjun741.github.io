---
tags:
    - 操作系统
    - Mac
---

Mac 下访问Windows(NTFS)分区的方法

http://www.makeuseof.com/tag/write-ntfs-drives-el-capitan-free/



使用命令行：

Once open, type sudo nano /etc/fstab and enter your password when prompted. You will be presented with an editor window for the fstab file.

![](/img-post/开发/操作系统/Mac/Mac 下访问Windows(NTFS)分区的方法.assets/aab2041cb9cd4b9ea12b95538008f7ab.png)

Type LABEL=NAME none ntfs rw,auto,nobrowse (making sure that you replace NAME with the name of your external drive) and press enter. Then pressctrl+o to save the file followed by ctrl+x to exit the editor window.

Next, eject your drive and then reconnect（在系统自动应用磁盘工具中找到对应分区，选择“装载”） it. The drive will no longer show in Finder, but can be accessed by returning to Terminal and entering open /Volumes.

![](/img-post/开发/操作系统/Mac/Mac 下访问Windows(NTFS)分区的方法.assets/310821c48ae041b58f641fd2be30241b.png)

In the window that opens, you will be able view your drive, as well as copy, edit, and drag files onto it. If you will be using the drive regularly, you can ensure faster access by dragging it to the sidebar or making an alias.

