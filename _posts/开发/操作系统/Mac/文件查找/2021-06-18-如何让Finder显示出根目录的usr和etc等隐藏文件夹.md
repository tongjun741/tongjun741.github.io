---
tags:
    - 操作系统
    - Mac
    - 文件查找
---

如何让Finder显示出根目录的usr和etc等隐藏文件夹

终端输入：

1 defaults write com.apple.finder AppleShowAllFiles TRUE
2 killall Finder

同理将上面的TRUE改回FALSE就可以恢复隐藏



http://www.cnblogs.com/ccdev/archive/2012/09/06/2673702.html

