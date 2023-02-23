---
tags:
    - 操作系统
    - Mac
---

OSX 禁用休眠 禁止 sleepimage 文件

要完全的，合法的禁止 sleepimage 文件，不但要设置 hibernatemode 0，还要禁止电源和电池情况下的节能设置。



也就是说，你要跑下面的命令才行：



sudo pmset -a hibernatemode 0

sudo pmset -a autopoweroff 0

sudo pmset -a standby 0

sudo rm /var/vm/sleepimage



http://bbs.feng.com/read-htm-tid-8096366.html



```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"wrap":"true","verticalAlign":"middle","textAlign":"start","textColor":"#111111","backColor":"rgb(254, 254, 254)","fontName":"微软雅黑","fontSize":"14px","value":"本帖最后由 sh2sg 于 2014-7-4 10:47 编辑\nOK，这个是逼出来的，因为之前用的是网上流传的土法来禁止生成 sleepimage，尝到了苦头，而且2次！\n大家知道 OSX 有几种睡眠模式，其中 hibernatemode 可以是 0 (传统睡眠方式，不生成 sleepimage 文件)，3 和 25 (Apple 称之安全睡眠方式，会生成 sleepimage 文件)，大家也都知道可以用 sudo pmset -a hibernatemode 0 来禁止那个内存镜像文件。\n以前这个命令一直可用，直到 OS X Mountain Lion v10.8.2，大家突然发现这个命令不起作用了，重启电脑或者睡眠一段时间后，那个 sleepimage 又回来了。换句话说，某些型号的 mac，似乎强制使用安全睡眠方式。\n然后网上就有各种土法，粗暴之极，诸如建立一个只读的空文件，或者用 sudo ln -s /dev/null /var/vm/sleepimage 把内存镜像引入系统黑洞。这些土法，一般用用大概没什么问题，就算有问题，你大概也不会太在意，譬如程序崩溃，大不了重新启动好了。\n我之前就是用 /dev/null 这个方法，然后我2次系统升级都出问题，2个月前 10.9.2 升级到 10.9.3 时，以及昨天 10.9.3 升级到 10.9.4 时，升级安装界面2次都停留在同一个地方，说还有几分钟就好了，然后，然后，就没有然后了，等了一个多小时，状态条动都不动。不得已强行关机。\n因为我没弄其他任何所谓的系统调试东西，唯一运行过的 sudo 命令就是这个，所以怀疑是这个土法导致升级出错。\n然后就觉得要花点时间弄明白 OSX 的睡眠方式，如果真不能禁止 sleepimage，死也要死得明白。\n以下是我的理解，绝对不同于各大中文 Mac 网站抄来抄去的那些东西。欢迎探讨。\nOSX 的睡眠参数，可以打 pmset -g 了解一下你的电脑处在什么睡眠模式下：\n比较有兴趣的参数：\nstandbydelay 10800\nstandby 0\nautopoweroffdelay 14400\nautopoweroff 0\nhibernatemode 0\n这几个参数组成了 OSX 的睡眠模式。\n当睡眠开始时，合上盖子，或者按电源键，如果你的 hibernatemode 是 0，OSX 是不立即往硬盘上写内存镜像的。\nautopoweroff 这是欧盟的节能要求，满足以下条件时：\n- 接电源\n- 没有外接设备\n- 没有网络活动\n- 电脑是 MacBook Pro (Mid 2012 and later)， MacBook Pro (Retina, Mid 2012 and later)， MacBook Air (Mid 2012 and later)， iMac (Late 2012 and later)， Mac mini (Late 2012 and later)\n到了 autopoweroffdelay x 秒后，就开始启动安全睡眠模式，往硬盘上写 sleepimage，然后进入深度睡眠。\nstandby 满足以下条件时：\n- 用电池\n- 没有外接设备\n- 没有网络活动\n- 没有外接显示器\n- 电脑是 MacBook Pro (Retina, 13-inch, Late 2012) and later， MacBook Pro (Retina, 15-inch, Early 2013) and later， MacBook Pro (Retina, Mid 2012)， MacBook Air (Mid 2010) and later， SSD and Fusion drive versions of Mac mini (Late 2012) and later， SSD and Fusion drive versions of iMac (Late 2012) and later\n到了 standbydelay x 秒后，就开始启动安全睡眠模式，往硬盘上写 sleepimage，然后进入深度睡眠。\n可见，Apple 的安全睡眠其实是个统称，具体是由2个参数激发的，这2个参数都可以在普通睡眠一段时间后让电脑进入深度睡眠状态，但是作用的条件不相同，基本上一个是接电源时用，一个是用电池时用。\n这也说明了为什么有人抱怨为什么在设置了 hibernatemode 0 后，睡眠了一段时间后，那个 sleepimage 文件又出现了，而有人说没有。这取决于他们各自睡眠的时间以及延迟时间的设定，合上又马上打开，那个文件是不会立即生成的。\n所以，要完全的，合法的禁止 sleepimage 文件，不但要设置 hibernatemode 0，还要禁止电源和电池情况下的节能设置。\n也就是说，你要跑下面的命令才行：\nsudo pmset -a hibernatemode 0\nsudo pmset -a autopoweroff 0\nsudo pmset -a standby 0\nsudo rm /var/vm/sleepimage\n然后不管你怎么重启，睡眠n久，都不会再生成内存镜像文件了，当然你的电脑就无法再进入深度睡眠模式，Apple 官方说电池待机能力可能会稍稍降低，但我看也未必，普通睡眠状态下电压已经非常小了。\n如果不在乎硬盘空间的，或许不用管它，用 0 即可，也就是普通睡眠了几个小时后才往硬盘写内存镜像文件，再进入深度睡眠。\n如果你也不在乎经常读写硬盘的，或者懒得折腾任何东西的，也可以用缺省模式，对笔记本来说是 3，也就是睡眠后马上就写内存镜像文件，再在几个小时后进入深度睡眠。\n怎么知道电脑进入了深度睡眠？就是唤醒时看到灰屏，和载入进度条。\nOSX 的深度睡眠看起来还是不错的，有机会可以跟人卖弄一下，它的深度睡眠，可以待机 1 个月。当然如果真的有人这么做，那这人肯定是缺心眼的了。"}],"widths":[744],"heights":[23]}
```





https://zhidao.baidu.com/question/1766181099205887900.html

如何彻底删除 Mac OS X 里那巨大的 sleepimage 文

用 SSD 的朋友硬盘空间不大，需要节省。而 Mac OS X 的冬眠模式会自动放一个和内存等大的名叫「sleepimage」的文件到 /private/var/vm 目录，换言之，你的内存是多少 GB，就有多少 GB 的硬盘空间会被这个文件吃掉。
以前介绍过用命令行方法禁用冬眠模式，只要在终端里运行 sudo pmset -a hibernatemode 0 即可。
但这招在 Mac OS X 10.7 (Lion) 上似乎行不通，禁用之后删除 sleepimage，过不了多久它又会死灰复燃。
我试过一个叫 SmartSleep 的软件，同样无效。
最后网上搜到一个偏方，问题解决。分享给大家。
打开 TextEdit，建立一个空的文本文件，取名为 sleepimage，保存到桌面。
去 Finder 里找到这个文件，选中，按 return，将后缀名（.rtf 或 .txt）删除。
在 Finder 里按 Command + Shift + G，输入 /private/var/vm，回车。新开一个 Finder 窗口，将桌面的 sleepimage 文件拖入上述第三步中的窗口。这时系统会要求你输入管理员账号密码。在 /private/var/vm 中选中 sleep image，按 Command + i，把 Locked 勾上。完毕。

