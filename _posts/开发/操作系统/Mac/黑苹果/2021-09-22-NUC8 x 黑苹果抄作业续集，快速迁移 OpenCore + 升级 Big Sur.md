---
tags:
    - 操作系统
    - Mac
    - 黑苹果
---

## 〇、前言

几个月前我曾写了一篇 **NUC8** 安装**黑[苹果](https://pinpai.smzdm.com/1687/)**的「抄作业」方案，包括硬件选购和软件安装，收到了很多值友的关注。 ![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/33.png)

[![img](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5f0423fc5e9b83250.jpg_a200.jpg)](https://post.smzdm.com/p/a9927x47)[**玩机技巧 篇十一：NUC8 打造 99%完美黑苹果，最简洁的抄作业方案**](https://post.smzdm.com/p/a9927x47)欢迎参加#果粉是怎样炼成的#征稿，围观秋季发布会新品！是什么让苹果生态无法割舍？快来讲讲你的果粉炼成记，AirPodsPro等丰厚奖品等你来！>点击这里查看活动详情[犹豫94想买](https://zhiyou.smzdm.com/member/9279924824/)|*赞*627*评论*330*收藏*3k[查看详情](https://post.smzdm.com/p/a9927x47)

当时，我使用的是 macOS **Catalina** 10.15.6 系统。今天凌晨（11月13日）苹果正式推送了 macOS **Big Sur** 11.0.1 系统，看到黑苹果玩家群有不少人都升级了，我也迫不及待的想试试。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/47.png)

> 这里先跟*之前的读者说声抱歉*，我上一篇的双系统教程其实有一点误区。因为我当时是装机过了几天才写的文章，细节记录略有出入。

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79db834442581.png_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_2/)

今天我就趁热打铁（中午刚升级的系统），写一篇黑苹果**抄作业续集**，顺便把之前**遗漏的部分**一起聊聊。需要注意的是，我一直有使用 NAS 给 Mac 的「**时间机器**」备份数据的，本文仅供参考，建议备份后再上车哦~![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/55.png)

> 特别感谢一下 NUC 玩家群 *@维奇* 大佬，我只是用自己擅长的文字，去*重新梳理*，并*实践*了一遍他和其他黑苹果技术大佬的技术成果。

京东[![img](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fed2748e6cd66379.jpg_a200.jpg)](https://go.smzdm.com/0cd7670c8cbd401e/ca_bb_yc_163_andr9030_11335_1687_179_0)[**QNAP 威联通 TS-251D 2盘位NAS（J4005、2GB）**](https://go.smzdm.com/0cd7670c8cbd401e/ca_bb_yc_163_andr9030_11335_1687_179_0)2299元起实时价格6小时前已更新[去购买](https://go.smzdm.com/0cd7670c8cbd401e/ca_bb_yc_163_andr9030_11335_1687_179_0)

万一觉得文章还不错呢，请三连支持下。我看看这期能否超过 13 个赞，今后我会给大家种草更多超酷的数码、软件、游戏和[智能家居](https://www.smzdm.com/fenlei/zhinengjiaju/)等产品。

## 一、迁移到 OC

如果你之前是跟我一样的安装方法，使用的应该是🍀 **Clover** 方式引导。而现在更主流的方式其实是🧿 **OpenCore**（简称OC）。两者理论上都可以用来安装（引导）黑苹果，我为什么要从 Clover 迁移到 OC？![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/107.png)

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79db7cf36812.png_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_3/)

简单来说，Clover 虽然发布早、受众广，配置相对简单直观，但它也背负了很多历史包袱，为了兼容性做出许多妥协。而 OpenCore 作为一款布局未来的开源模块化工具，可以实现更接近原生体验，维护也方便，但缺点就是目前的配置繁琐，不适合新手。

不过，既然是抄作业，（仅针对 **NUC8** 这款机型而言）大佬们早已准备好了**完整的配置模板**，跟我一样的**小白依样画葫芦**就行了。于是，我最终决定迁移到 OC 再升级 Big Sur 更稳妥。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/74.png)

京东[![img](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5e61b592328719703.jpg_a200.jpg)](https://go.smzdm.com/26554722a8e6936f/ca_bb_yc_163_andr9030_11335_1687_179_0)[**intel 英特尔 NUC系列 NUC8i5BEH6 迷你台式机 酷睿i5-8259U 核芯显卡 黑色**](https://go.smzdm.com/26554722a8e6936f/ca_bb_yc_163_andr9030_11335_1687_179_0)2599元实时价格6小时前已更新[去购买](https://go.smzdm.com/26554722a8e6936f/ca_bb_yc_163_andr9030_11335_1687_179_0)

### 1.替换文件

首先，我们打开 **Clover Configurator**，挂载本机 EFI 分区，顺便备份一下原EFI[文件夹](https://www.smzdm.com/fenlei/wenjianjia/)。

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79dba1e012183.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_4/)

~~然后删除原 EFI 文件夹的全部文件（即 Clover 引导），再解压*@维奇* 大佬制作的 **OC-EFI.zip**，把新的文件（OC引导）全部放入 EFI 分区，然后打开 Readme.txt 看一下重点提示。~~

*维奇*的EFI要收费，在github上找了一个替代品：

https://github.com/zearp/Nucintosh

> 注意：*双系统*用户请保留 /EFI/Microsoft 文件夹，并使用对应的 plist 文件。

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79dc1daec4578.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_5/)

### 2.迁移三码

这一步推荐用*@豪客88* 大佬制作的三码迁移工具（**MacSerialTransporter**），操作简单，比较适合小白。【注意：MacSerialTransporter 只能运行在10.15以上。如果没有特别需求的话重新生成三码就可以了：最简单的图形化操作，可以使用 OOC来配置，具体的操作方法可以参考：https://blog.csdn.net/lxyoucan/article/details/110730680这篇文章搜索 “机型设置”。】

> 三码指序列号、[主板](https://www.smzdm.com/fenlei/zhuban/)序列号、系统UUID，一般用于提供身份令牌，使用户可以正常使用 iMessage、Facetime 等服务。因为我是从 EFI 迁移到 OC，这个三码也需要一并迁移。

左边选择刚才备份的/EFI/CLOVER/config.plist，右边打开挂载EFI分区的/EFI/OC/config.plist，最后点击中间的>>符号完成迁移。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/134.png)

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79dc0fc2c6215.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_6/)

### 3.修复启动

修改完成后就可以重启，然后使用新的 OC 引导开机了……不过首次用 OC 引导进入系统前建议**清空 NVRAM**，具体操作如下。

- *单系统*用户：按开机键后，连续点击 *ESC 键*进入 OC 引导界面，按空格显示隐藏选项
- *双系统*用户：按开机键后，默认进入 OC 引导界面，按*空格键*显示隐藏选项
- 接着选中最后一项「*Reset NVRAM*」，回车键执行，然后重启即可

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79dc41b5f3133.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_7/)

但是，貌似我是双系统的缘故，重启后**找不到启动项**，这时候就需要使用 PE 系统**修复引导项**——启动正常的可以跳过下面的部分。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/51.png)

我们借助一个启动 U 盘进入 **PE 系统**（以 WEPE 为例），首先挂载 ESP 分区（即 EFI 文件夹），然后打开「扇区小工具BOOTICE」新增一个启动项，从 EFI 文件夹里选择**EFIBOOTBOOTX64.efi**，移至首位，保存并重启。

> 这也是我*上一篇教程*没讲清楚的地方：对于*初次安装双系统*的朋友，先安装 macOS，然后安装 Windows，接着挂载 EFI 分区再*覆盖一遍 Clover/OC 的引导文件*即可；当然也可以用 PE 如法炮制。

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79dca89b8974.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_8/)

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79dd1b1d55002.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_9/)

## 二、升级系统

成功更换 OC 引导进入系统后，我们就可以开始下载 Big Sur 系统了，升级文件比较大（12.18GB），我家里是联通 100M 宽带，大概用了 50 多分钟才下完。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/76.png)

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79dce36c36382.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_10/)

安装系统的基础操作我不再赘述，按提示下一步即可。整个安装过程大概需要**半小时**，中途如果遇到**黑屏**了不要急，试试动一下键盘鼠标；如果依然没反应就重启，选择「**macOS installer**」启动项继续安装。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/35.png)

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79dd4e4513416.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_11/)

## 三、快速体验

因为我是从 10.15.7 直接升级 11.0.1 的，进入系统后首先会提示「**优化性能**」，稍微卡顿几分钟属于正常现象。整体来说，我的 NUC8 升级到 macOS Big Sur 后，使用过程中基本**很流畅**。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/64.png)

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79ddd84365979.png_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_12/)

要说升级 Big Sur 以后最直观的感受，那肯定就是焕然一新的**外观设计**了。通知中心、小组件、Dock栏以及多个系统应用的界面都与 iPadOS 的风格更加接近。

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79ddeba526192.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_13/)

早在测试版发布时，就有很多网友吐槽过这套新图标，重回**拟物风格**，但**阴影**、**色调**都偏浓重了。至于好看与否，这就见仁见智了，反正总会习惯的……吧？![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/99.png)

> 说实话用了一天 Big Sur 之后，我都有点忘记原来的 Catalina 长什么样了。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/57.png)

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79dde3ad93895.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_14/)

因为我也是刚升级的系统，体验时间有限，简单提几个我关注的点吧。

- NUC 8 的各项*系统功能*均正常，几乎和之前 Catalina 一样
- 部分应用程序的*启动速度*貌似变快了，即点即开，很少图标弹几下
- AirPods *智能切换*效果不算很完美，但连接速度比之前快多了

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79de607248021.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_15/)

对于我来说，在 **Big Sur 11.0.1**系统下，我常用的**工作流**需求是完全 OK 的，所以用着挺舒服。然后下面是我自己遇到的、搜集到的一部分**软件兼容性**问题：

> 没提到的常用软件基本是正常的，比如 Office、PS 2020、VS Code 等。

- *QQ 体验版* v8.3.6 无法使用……*通用版*正常
- *[爱奇艺](https://pinpai.smzdm.com/37404/)* v11.11无法打开……*卸载重装*可解决
- *PD 虚拟机* v16 能用，但部分汉字无法显示
- Final Cut Pro 盗版无法使用

[![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/5fae79de78cd32161.jpg_e1080.jpg)](https://post.smzdm.com/p/andr9030/pic_16/)

## 四、结尾

总结一下，借助神奇的 **OpenCore** 引导，NUC8 这台黑苹果可以直接从 Catalina 升级到最新的 **Big Sur**，系统功能均正常，没有什么明显 Bug。不过这才是正式版的第一次更新，还有许多值得优化的空间，不着急的朋友可以等等。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/106.png)

这里顺便说说我对 **ARM 架构 Mac** 的看法，虽然搭载 M1 芯片的 Mac 新品还没发货，但从介绍资料来看，我还是有点期待的。我大概还会使用这台 NUC8 玩黑苹果**2~3年**吧，正好也是 ARM 新平台的**过渡期**，到时候看情况吧，我可能会换一台 **Mac mini**。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/119.png)

> 2~3年后如果想继续使用 *Intel Mac* （黑苹果）理论上也是 OK 的，直到苹果*停止更新维护*（自家旧设备）的那一天。

京东[![img](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/60dad9cb43d598034.jpg_a200.jpg)](https://go.smzdm.com/a32c5e6b39e00a6c/ca_bb_yc_163_andr9030_11335_1687_179_0)[**Apple 苹果 Mac mini 2020款 台式机 银色(Apple M1、核芯显卡、8GB、256GB SSD、风冷)**](https://go.smzdm.com/a32c5e6b39e00a6c/ca_bb_yc_163_andr9030_11335_1687_179_0)5299元实时价格6小时前已更新[去购买](https://go.smzdm.com/a32c5e6b39e00a6c/ca_bb_yc_163_andr9030_11335_1687_179_0)

还是那句话，黑苹果终究不会是完美的，而且仅仅是**小众**的玩物。最后祝大家搞♂机愉快，有问题可以留言交流。

------

> 如果觉得本文对你有帮助，欢迎「➕关注➕点赞➕收藏」鼓励一下新人生活家。![NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur](/img-post/开发/操作系统/Mac/黑苹果/NUC8 x 黑苹果抄作业续集，快速迁移 OpenCore + 升级 Big Sur.assets/129.png)
>
> 以后我会分享更多有趣的数码、游戏、家居好物和玩机技巧，让小白也能享受科技的乐趣。

最后是惯例的 GIF 小彩蛋环节，「*问了就是想要*，**犹豫94想买**」，我们下次更新见~

[
](https://post.smzdm.com/p/andr9030/pic_17/)