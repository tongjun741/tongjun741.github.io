---
tags:
    - 其它
    - 虚拟机
---

随着VMware虚拟机使用时间的增长，其所占用的空间也越来越大，本文来说说怎么给VMware虚拟机占用的空间进行瘦身。

方法一：VMware自带的清理磁盘
这个方法是VMware自带，具有普适性，对快照等文件不造成影响。
步骤如下：
1、将要清理的虚拟机关机。
2、右键该虚拟机——>管理——>清理磁盘，VMware会自动提示可清理的磁盘大小，点击确定等待清理完毕即可。

![img](/img-post/开发/其它/虚拟机/VMware对macOS虚拟机硬盘瘦身.assets/846050-20190710103334359-1587084463.png)

方法二：VMware自带的碎片整理和压缩
这个方法也是VMware自带，具有普适性，对快照等文件不造成影响。碎片整理花费时间可能比较长，有个心理准备。
步骤如下：
右键该虚拟机——>设置——>硬件——>硬盘——>碎片整理，整理完成后，点击压缩。

![img](/img-post/开发/其它/虚拟机/VMware对macOS虚拟机硬盘瘦身.assets/846050-20190710103355613-2112730751.png)

方法三：虚拟机另存为OVF文件，清空原有盘
经过长期的VMware使用我发现，有时候删除虚拟机快照出现错误，但快照图标已消失，导致无法再次删除，造成文件残留，就这样越堆越多，无法清理。
这个方法属于杀手锏，在其他方法效果不大的时候使用，比较适用虚拟机空间极度需要清理的情况。
优点是可以释放大量空间，缺点是只能保留VMware虚拟机当前的状态和文件，丢失其他快照（可以按需先转到某个快照再导出OVF，这样就可以保留快照时的状态了。同样，会丢失其他状态）。

步骤如下：
1、点击要清理的虚拟机，然后左上角点击文件，导出为OVF（只存了虚拟机当前的状态，大概有十几个G），存到其他空闲的磁盘下。
2、将上述步骤导出的ovf再部署出来，看看虚拟机是否正常。
如果正常可用，就可以把虚拟机原来占用的磁盘清空了，快速释放大量空间。
如果虚拟机不正常，试试重新导出OVF。

![img](/img-post/开发/其它/虚拟机/VMware对macOS虚拟机硬盘瘦身.assets/846050-20190710103409405-379455980.png)



https://www.cnblogs.com/dingjiaoyang/p/11162299.html