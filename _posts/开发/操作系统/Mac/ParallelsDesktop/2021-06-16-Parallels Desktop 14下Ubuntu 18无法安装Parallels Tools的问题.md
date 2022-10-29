---
tags:
    - 操作系统
    - Mac
    - ParallelsDesktop
---

Parallels Tools

安装完虚拟机之后，你还需要在虚拟机中安装`Parallels Tools`，它实际上就是虚拟驱动。

安装步骤如下：

1、通过点击菜单`Actions ➝ Install Parallels Tools...`，加载`prl-tools-lin.iso`镜像文件。

2、在虚拟机中`mount`该镜像文件：

```
sudo mount -o exec /dev/cdroom /media/cdroom
```

3、执行安装操作：

```
sudo /media/cdroom/install
```

4、安装完成后，会提示您重启。

**注意：**有时候会安装出错，主要原因是`prl-tools-lin.iso`比较老，较新的系统使用了高版本的内核， 导致某些内核API发生变化，从而出错。解决的办法就是下载较新的安装包（ParallelsDesktop15+），从中提取出新的`prl-tools-lin.iso`，把旧的替换掉。 步骤如下：

1、下载最新的安装包`ParallelsDesktop-*.dmg`

2、使用[hdiutil](http://blog.fpliu.com/it/os/macOS/software/hdiutil)打开`ParallelsDesktop-*.dmg`：

```
hdiutil attach ParallelsDesktop-*.dmg
```

3、进入`/Applications/Parallels\ Desktop.app/Contents/Resources/Tools`目录：

```
cd /Applications/Parallels\ Desktop.app/Contents/Resources/Tools
```

4、备份旧的`prl-tools-lin.iso`：

```
sudo cp prl-tools-lin.iso prl-tools-lin.iso.bak
```

5、替换新的`prl-tools-lin.iso`：

```
sudo cp /Volumes/Parallels\ Desktop\ 15/Parallels\ Desktop.app/Contents/Resources/Tools/prl-tools-lin.iso .
```

6、进入虚拟机，重新走一遍安装流程。即可成功安装。



http://blog.fpliu.com/it/software/Parallels