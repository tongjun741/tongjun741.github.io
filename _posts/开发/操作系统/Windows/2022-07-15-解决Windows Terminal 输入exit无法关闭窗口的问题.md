---
tags:
    - 操作系统
    - Windows
---

现象：

```
ping baidu.com

# 按下ctrl+c 结束命令

exit
```

此时窗口不自动退出



解决方法1：

在Windows Terminal中进入设置窗口，点击”打开JSON文件“，修改profiles.defaults.closeOnExit为"always"

```
{
    "profiles": {
        "defaults": {
            "closeOnExit": "always"
        },
    }
}
```

重启Windows Terminal



解决方法2：

Added an alias called "exit" on my .bash_profile: alias exit="exit 0"



https://github.com/microsoft/terminal/issues/4573