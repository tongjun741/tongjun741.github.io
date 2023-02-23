---
tags:
    - 操作系统
    - Windows
---


intel第11代处理器(Intel Tiger Lake)，采用新的Intel Volume Management Device（VMD）技术，可以优化储存装置的数据处理效率与耗电量。搭载11代U的机器在安装windows10时会遇到找不到硬盘的情况。
如果主板BIOS界面中有VMD设置项，也可以在BIOS中将VMD功能关闭
没有的话，只能按照下面的方法安装.

![img](http://www.zylxyl.com/wp-content/uploads/2020/10/20201023192948.jpg)

当您安装Windows 10零售版/企业版本操作系统时，都需要在安装过程中加载IRST 驱动程序才能正常安装。
如果您的计算机是英特尔第11代处理器(Intel Tiger Lake)，且遇到安装Windows 10的过程中无法找到驱动器（硬盘），可以参考以下步骤加载IRST驱动来解决问题。

点击下载 [Intel Rapid Storage Technology (IRST)](https://dlcdnets.asus.com/pub/ASUS/nb/DriversForWin10/Others/V18.0.4.1146_IRST_VMD_20H1.zip) 驱动程序，并将其复制到安装U盘，（压缩包，需要解压缩至U盘），在Windows10安装界面，点击加载驱动程序，选择解压缩的文件目录，按提示信息安装即可（Intel RST VMD Controller 9A08 (TGL)）。



http://www.zylxyl.com/archives/2020/10/23/1206.html