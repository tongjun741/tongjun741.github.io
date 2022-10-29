---
tags:
    - 操作系统
    - Windows
---

最近使用使用windows terminal的的时候，想起来，它已经自己自动集成了4种命令行，我每次打开git的时候是单独的，很不方便，想到集成到一起会比较方便管理和使用。便研究了一下，配置起来很简单。

依次点击：下拉框 → 设置 → 添加新配置文件 → 新建空配置文件 → 常规
然后就可以自定义了
我的配置文件的git安装home目录是`D:\Program Files\Git`，按需修改

名称：`Git Bash`
命令行：`D:\Program Files\Git\bin\bash.exe`
启动目录：`%USERPROFILE%`
图标：`D:\Program Files\Git\mingw64\share\git\git-for-windows.ico`

保存就可以了

常见问题：

1. 目录包含中文时不能正常显示
2. 不能使用`ll`命令

使用使用 `Windows terminal` 打开 `Git Bash`
修改文件：`vim /etc/bash.bashrc`

```
# 设置windows terminal 打开bash时支持显示中文目录
export LANG="zh_CN.UTF-8"
export LC_ALL="zh_CN.UTF-8"
# 让ls和dir命令显示中文和颜色 
alias ls='ls --show-control-chars --color' 
alias dir='dir -N --color' 
# 设置为中文环境，使提示成为中文 
export LANG="zh_CN" 
# 输出为中文编码 
export OUTPUT_CHARSET="utf-8"
# 设置 windows terminal打开bash时支持ll命令
alias ll='ls -l'
```



terminal版本

> Windows Terminal
> 版本: 1.11.3471.0

git版本

> git version 2.32.0.windows.1

[git](https://www.msezi.com/tag/git)

https://www.msezi.com/293.html



可能会遇到以下几个问题：

gitbash的gitconfig文件修改时提示拒绝访问的解决：https://blog.csdn.net/sxf359/article/details/109150986

Windows Terminal 闪屏解决办法,以及使用细节：http://www.cncsto.com/article/201513