---
tags:
    - 操作系统
    - Linux
    - 查找文件
---

mac linux rename命令行批量修改文件名

我的mac使用命令行批量修改名字时发现居然没有rename的指令：

zsh: command not found: rename

所以使用HomeBrew先安装一下：

➜  ~ brew install rename

完后可以直接使用简单的一行命令进行多个文件的命名修改，大致格式如下：

➜  ~ rename 's/old/new/' *.files 

例:

修改批量的png文件的前缀由'ic_'改为'ic_setting_' :

(ic_launcher.png -> ic_setting_launcher.png)

➜  ~ rename 's/ic_/ic_setting_/' *.png 


修改批量文件的部分命名：

(ic_setting_launcher.png -> ic_launcher.png)

➜  ~ rename 's/_setting//' *.png 


用法都差不多，剩下的就靠我们自己灵活运用了。



