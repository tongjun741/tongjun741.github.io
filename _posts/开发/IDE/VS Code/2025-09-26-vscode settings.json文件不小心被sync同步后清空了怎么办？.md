---
tags:
    - IDE
    - VS Code
---

我的vscode登录了GitHub，并且开启了sync同步功能，所以在其他新的设备上下载vscode然后登录GitHub就可以同步我常用的设置了。

但是，最近我在Ubuntu系统上下载了vscode，并且登录了GitHub进行同步，在同步的过程中说发现了冲突，我并没有细想就随便操作了下。然后就同步成功了。

然而，我在回到我Windows端上打开vscode时发现我的一些基本配置都没了，我打开settings.json文件后发现这个文件被还原了。瞬间有点懵逼。。。

经过查阅资料，以及各自摸索后，终于在settings.json这个文件的文件夹下，我发现了一个名为History的文件夹：

点开这个文件夹，我发现了很多文件，然后找到离今天最近的那次修改日期的文件，我找到了还原前的settings.json文件（文件名是随机的），然后复制粘贴到现在的settings.json文件后，我的配置都回来了。Thanks for god!



![image-20250926175303756](/img-post/开发/IDE/VS Code/vscode settings.json文件不小心被sync同步后清空了怎么办？.assets/image-20250926175303756.png)

![image-20250926175334168](/img-post/开发/IDE/VS Code/vscode settings.json文件不小心被sync同步后清空了怎么办？.assets/image-20250926175334168.png)

这篇博客就记录下这个坑，希望遇到同样问题的朋友可以有启发。



https://blog.csdn.net/qq_32614873/article/details/129561449