---
tags:
    - NAS
---

## **前言**

自从国庆节之后折腾黑群晖以来，不知不觉已经过去了近一个月，这个月虽然几乎天天呆在家，但仍旧过得很充实。其实这就是折腾的意义，不求真的得到最完美的结果，而是从学习的过程中获得快乐。第一次写的系列文章也不知不觉来到了第四篇，感谢前几篇中点赞、收藏和评论的各位值友，NAS的解决方案有很多种，衍生出来的玩法更是千姿百态，我始终相信明确自己的需求并通过各种方案达成，就是最合适的。愿我的这一系列文章能够给您带来一些思路的参考，欢迎各位值友一起交流讨论。（**该系列其他文章请戳原文。**）

废话不多说，下面进入正题。这一篇的内容难度有所提高，所占篇幅也比较大，所以临时从入门篇中提取出来做成了进阶篇，新手写教程难免有出错或者不全面的地方，还请各位大神指正。

## **一、关于群晖的下载管理**

其实群晖自带的下载器Download Station也挺好用的，支持几乎所有常用的下载协议，而且还能监控RSS自动下载，功能是蛮强大的。但是因为我同时有PT/BT下载的需求，同一个软件肯定无法满足，所以必须安装第二个下载软件。迅雷远程在去年封闭了第三方接口之后，已经算是自断了双腿，而迅雷替代者中呼声最大的当属号称下载神器的Aria2了吧。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-ccec421c415706e7b4e96cae70f12e42_720w.jpg)

100M电信跑出150M的带宽

## **二、Docker 中安装下载软件Aria2及其设置**

## 1、什么是Aria2？

Aria2是一款基于命令行的超轻量级全平台多协议下载工具，支持诸如HTTP/FTP/BT/磁力等下载协议，唯独不支持电驴（在此缅怀一下当年的VeryCD ）。Aria2本身是不带操作界面的，所以叫做命令行工具，但为了方便使用，很多大神自制了UI界面，常见的有Aria2WebUI、AriaNg等。这两种界面都是基于网页的，所以只要能连接到Aria2的服务器，无论在什么地方都可以轻松的进行下载管理，实现远程下载。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-80eac2912952fc771bb06ce7f1602f10_720w.jpg)

Aria2WebUI的界面



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-2e2c6391bcced68ceaa63c631fc347a3_720w.jpg)

AriaNg的界面

群晖的套件中心里是没有Aria2的，但群晖有Docker套件，可以在Docker中进行安装使用。至于Docker是什么，这里就不展开说了，我也不是很懂，就把他当一个虚拟机来看待就好，随意折腾，不会影响到群晖的系统。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-2465e528b8f68c6ad26984f69872a55b_720w.jpg)

图片来源于CSDN

## 2、安装Aria2

首先在套件中心中安装Docker，安装好之后打开。第一次进入Docker的界面是这样的，什么都没有。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-03bf97c4687d4fe31d859de29da377c7_720w.jpg)



点击左侧的注册表，使用关键字查找Aria2，可以看到有很多不同版本。这里我选的是第一个xujinkai/aria2-with-webui，从下面的注释可以看出来，这个镜像包含了Aria2和webui，这样使用的时候有图形界面会比较方便。选好之后点击上方的下载。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-8f61f20bb7e38b433844e9c8ecc3e4a7_720w.jpg)



此时左侧映像标签有个小1标识，点击映像，看到此镜像正在下载。Docker的镜像下载服务器对国内网络的支持不是很好，有时候下载很慢，请耐心等待。好在这个镜像大小只有30M。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-3edf83349c91184e8cac777040b2414b_720w.jpg)



下载完成后选中映像点击上方的启动按钮，开始创建容器。第一页的容器名称可以任意填写，高级权限没有必要选，资源限制可以根据自己的实际情况来，我这边没有做限制。下面点击高级设置。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-73a39105ce622825ed2b50e74ef32abc_720w.jpg)



第一个页面常规设置，这里建议将启用自动重新启动打开，创建桌面快捷方式根据自己需要，可以将快捷方式指向webui的网址用于打开Aria2的下载界面。



![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='600' height='509'></svg>)



选到第二栏卷标签，需要在此标签页添加两个文件夹。点击添加文件夹，会弹出一个选择路径的窗口，选择一个文件夹作为默认下载文件夹，在对应的装载路径中填入/data。再次添加文件夹，选择一个文件夹作为Aria2的配置文件储存位置，然后在装载路径中填入/conf。这里前边黄框里的文件夹位置表示的是你自己的NAS中的实际文件夹路径，后边绿框里的文件夹位置表示的是程序识别的下载和配置路径。绿框中的装载路径是镜像作者设置的固定路径，所以必须按这个填，否则会导致程序无法开启。若选择其他镜像可以到镜像说明网页中查看特殊要求。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-d09d1e727e526a21e68dbfdcfdc4b9ae_720w.jpg)



网络标签页保持默认不用修改，端口设置页建议将本地端口从自动改成固定的端口号，这个是任意填写的，需要记住稍后有用。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-7e8dce321615e5df4102a01c760eedbd_720w.jpg)



环境标签页中点击+号添加一个，可变填入rpc-secret，值中任意填写一个验证码，这个在连接Aria2时需要用到。到这里高级设置就完成了，点击应用，回到上一级页面之后点击下一步。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-642c511fd1d279e1189ea40d44ee89f2_720w.jpg)



这一页会把你的设置全部展示出来，检查没有问题就可以点应用了，默认向导完成后运行此容器。



![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='600' height='429'></svg>)



现在Aria2下载器已经在运行了。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-a4c30ca83acbf47fb31b3defd6277936_720w.jpg)



## 3、使用Aria2

在浏览器中输入群晖的IP和刚才设置的端口号，进入Aria2的WebUI界面，我的地址是192.168.1.235:6880。如果想将快捷方式直接指向这个页面，就将地址填到快捷方式栏中。此时页面提示连接RPC认证服务器失败，无法连接。点击设置，选择连接设置。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-a5abd122fdc408d7ff621ca3b5a4c8d8_720w.jpg)



在密码令牌处填写刚才设置的认证口令，这时RPC认证通过，会提示连接成功，左侧的设置窗口也会把相关配置显示出来。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-67d8331da43ae605cef40ca664a5f288_720w.jpg)



此时已经可以开始下载了，点击添加，可以通过连接、种子和磁力链三种方式创建下载。找一个HTTP链接测试一下。界面上方蓝框处有链接的添加规则，下载设置中的dir表示的就是下载地址，可以手动修改，但注意只能在/data后面加文件夹，如/data/TV这样，因为这个/data就是安装时在绿框中填入的让Aria2识别的一个路径地址，其对应的则是黄框内的NAS实际文件夹地址/HDD WDR/Downloads。在此处没有类似文件夹管理器的修改界面，只能通过文字输入文件夹地址。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-bbb5c56a2785f82fc226a4e3bd617447_720w.jpg)



下载了一个WIN10系统的镜像文件，我家100M的电信宽带跑出了18MB/s的平均下载速度，比理论满速12.5MB/s还快了近50%。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-dda60acf3eb114875c5463d2fe2c3263_720w.jpg)



## 4、用Aria2做BT/磁力下载

BT和磁力本质上都是P2P，所以放在一起说。Aria2毕竟是命令行软件，不像迅雷、旋风之类什么都已经设置好了，尤其是要BT下载，是需要动手配置的。所以在百度上搜索“Aria2 BT没速度”，可以看到有11万8千个搜索结果。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-9bdda8c9a20d6feb12658eac0e3aa145_720w.jpg)



**4.1、配置Aria2**

修改Aria2配置的方法有两种，一种是在WebUI界面中点击设置——全局设置，会弹出新的窗口。所有的设置参数一股脑全都展现出来，滑动鼠标滚轮可以向下翻NNN页。如果觉得想要设置的内容不好找，上面有搜索框可以速度找到需要设置的参数（图中黄框）。如果不知道设置参数的含义，每一项参数后面还有该参数的解释（图中蓝框），不过是英文的，需要一定的阅读理解能力。反正我是无视的



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-f68ac967a5ff009c954d6d3a62fcea2d_720w.jpg)





![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-c651146a80f63ecdc6c02970e9c5f740_720w.jpg)



在全局设置中修改了设置之后，一定记得要用鼠标滚轮翻页到最下面点保存，不然改了没用



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-b59c14658f3cca9767c87b2beac60fe8_720w.jpg)



另一种方法，直接用写字板修改Aria2的配置文件。在NAS中找到创建Aria2时选定的配置文件地址，即黄框中的docker/config，aria2.conf就是配置文件。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-d09d1e727e526a21e68dbfdcfdc4b9ae_720w.jpg)





![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-518f41742e23e59f9005a4d8dc7a6568_720w.jpg)



将配置文件下载到本机后，使用写字板打开，可以看到一大段类似代码的东西，这就是Aria2的配置参数。头大吗？嗯，头大就对了，我看了也头大



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-5b30f737cf51423142960602e36cab76_720w.jpg)



不过没有关系，网上有非常详细的配置说明，下面的文章就很不错，直接把他的配置复制过来再根据提示修改就好。注意命令行前面如果有#的则不能生效，要起效必须将#去掉。（其实我也可以把我配置好的文件分享出来，但是好像原创中不能贴网盘或者上传文件。具体请戳原文。）

将配置文件内容复制到aria2.conf中，看上去稍微没那么恐怖了。红框中前面带#的中文就是下一行参数的解释，黄框中那种前面不带#的代码是可以生效的，蓝框中那种前面带有#的代码是不会生效的。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-6c47a61fabcc24f77b7574c1df4b3e45_720w.jpg)



**4.2、BT下载相关配置**

如果能弄懂上面的配置方法，那么恭喜你，Aria2对你来说就没有难度了。下面就针对BT下载来修改几个必要的相关参数。

首先需要明确的是，你要用Aria2下BT还是PT。因为BT是开放环境的P2P，需要跟迅雷等流氓软件抢连接，所以要设置的凶狠一点；而PT是封闭环境的P2P，只要在封闭环境中与其他人建立连接，没有各种流氓软件抢线，所以要设置的温柔一点。下面的各项参数都会按BT和PT分别提供两种设置建议。

> \1. # 单个种子最大连接数, 默认:55
> bt-max-peers=999
> 此项无论BT还是PT都建议设到999，要想下载速度快，种子连接多多益善
> \2. # 打开DHT功能, PT需要禁用, 默认:true
> enable-dht=true
> 此项PT下载必须设为false，否则有封号风险。BT下载务必设为true，跟流氓软件抢连接全靠它。
> \3. # 本地节点查找, PT需要禁用, 默认:false
> bt-enable-lpd=true
> 此项PT下载必须设为false，否则有封号风险。BT下载可以设为true，个人认为提升连接的能力并不强，但总好过没有吧。
> \4. # 种子交换, PT需要禁用, 默认:true
> enable-peer-exchange=true
> 此项PT下载必须设为false，否则有封号风险。BT下载务必设为true，可以连接到更多种子。
> \5. # 客户端伪装, PT需要
> \#peer-id-prefix=-TR2770-
> \#user-agent=Transmission/2.77
> 此两项PT下载需要将前面的#去掉使其生效，BT下载保留#。因为目前大多数PT站点的规则中不认可Aria2作为下载软件，所以PT下载时需要将Aria2伪装为Transmission。

以上参数按需要修改，其他的保持默认即可。

如果是用Aria2做BT下载，那么还需要在配置文档的最后增加一个参数：bt-tracker=。此参数是为Aria2提供额外的tracker服务器，从而让Aria2有机会建立更多的连接，从而提升下载速度。tracker服务器是一个个的网络节点，储存着所有下载者的下载信息，理论上连接越多的tracker也就意味着连接到更多的种子。

但怎么找tracker服务器呢？难道用百度？别急，这边提供一个每天更新tracker列表的网页给你。[点我进入](https://link.zhihu.com/?target=https%3A//github.com/ngosang/trackerslist)

无视整页的英文，翻页到下面找到Tracker List，这边每天会自动更新tracker列表，还会区分HTTP、UDP、ip和域名类的等等，但其实只要用到tracker_best的20个就够了。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-eea2591cd2f378c9171c5377bb2a3b3e_720w.jpg)



点击红框里的链接，会跳转到新页面或下载一个txt文件，没关系，把页面或者文件中的网址全部复制下来，粘贴到Aria2配置文档中的bt-tracker=后面，然后把每个网址之间用**英文输入法**的逗号隔开，注意必须是**英文输入法**，如图



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-295cb17370a298fd840b8cb1ac804e95_720w.jpg)



至此，Aria2的配置文档修改完毕。保存文件后，将此文件重新放回NAS中覆盖原文件。然后在docker的容器中将Aria2关机再启动，即可使修改好的配置生效了。可以点击使用右侧红框内的按钮控制关机和开机。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-955824c78b3b8d2d71e2681c9336701f_720w.jpg)



**4.3、BT下载隐藏技巧**

这个技巧是在[Senraの小窝](https://link.zhihu.com/?target=http%3A//www.senra.me/)原创文章：《[解决Aria2 BT下载速度慢没速度的问题](https://link.zhihu.com/?target=http%3A//www.senra.me/solutions-to-aria2-bt-metalink-download-slowly/)》中看到的，这里直接引用原文了。

> 上面提到DHT有缓存，是这样滴，和很多BT客户端一样，Aria2有个dht.dat文件(开启ipv6还有个dht6.dat)，这玩意用于存储一种叫做DHT Routing Table的东西，DHT网络由无数节点组成，你接触到一个后能通过它接触到更多的节点，Aria2我记得是有内置的节点，但是！如果你在Aria2第一次运行的时候直接下载磁力链接或者冷门种子，你很可能遇到连MetaData都无法获取的情况，这就是因为第一次只是初始化dht.dat文件，你本地不存在DHT Routing Table的缓存，所以你无法从DHT网络中获取足够的数据。
> 那么怎么办？我的建议是，找个热门种子(千万建议是种子，而不是磁力链接)，然后下一波，挂着做种，过几个小时后退出Aria2，或者等Aria2会话自动保存，你会发现dht.dat从空文件变成有数据了，这时候你下载就会正常很多。

照这样设置和操作过后，BT下载应该能跑满全速了。

## 5、玩转docker映像

在网上找到的大部分教程，都只会告诉你怎么安装docker映像和容器，告诉你在启动容器时某个地方怎么填怎么设置，却不告诉你为什么。那如果你不喜欢这个界面，或者下载不动这个映像，或者别的任何原因要用别的映像怎么办？下面就教你玩转docker映像。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-cd8eb253bed9e3503697d73530a4e3b7_720w.jpg)



首先看到docker中的注册表，可以看到每一个映像右边都有一个蓝色的箭头小标。点击这个小标会打开一个新的网页，这个网页就是这个映像制作者提供的说明页面。因为我想体验一下AriaNg的界面，所以这边以一个叫colinwjd/aria2-ariang的映像为例来演示。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-ff3ed8e9a244f24c894344d2631b6b2a_720w.jpg)



点击右侧蓝色箭头小标后，打开了这个映像的说明网页。虽然是英文，但大部分内容是映像的说明和安装方法，不用太在意，直接看到后面的内容。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-3f643b916b531c881b63383231f145e7_720w.jpg)



看到docker run之后的内容。基本可以看出来跟启动映像时设置的内容是匹配的。--name匹配容器名称，-p匹配高级设置中的端口设置，-v匹配卷，-e匹配环境。这里需要关注的主要就是-v和-e。也就是说卷中需要创建两个文件夹，关联到容器的/DOWNLOAD_DIR（下载目录）和/CONFIG_DIR（配置文档目录），环境中需要新建一个变量SECRET，对应的值由自己设定，这个就是连接到Aria2服务器的认证码。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-1087cf4843f0f63b342a61253c065047_720w.jpg)





![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-462347868a98b48dcc86f89d49b633b1_720w.jpg)



在卷和环境中按页面说明中的要求设置好，端口也设置好之后，就可以启动这个容器了。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-d419eeada3baf75802d47d45709ea8ba_720w.jpg)

AriaNg的界面比WebUI显得更容易接受一点

如果想玩其他的映像，也可以像上面这样通过映像的说明页面了解到需要设置的参数，然后按要求设置之后就可以正常使用容器了。

## **三、Docker 中安装下载软件Transmission及其设置**

## 1、什么是Transmission

照例先介绍一下Transmission，这是一款开源的BT下载软件，支持BT下载和磁力下载，主要支持Linux和Mac OS 操作系统（很遗憾没有windows版），这款软件最大的特点就是在保证功能的同时做到了对资源占用的极小化。大多带USB的路由器挂PT就是用的他，我之前在网件6300V2上用他下PT跑满全速没压力。

Transmission自带有WebUI界面，但个人感觉不好看，而且下载信息看得也不是很全面。个人比较喜欢使用另一款软件Transmission Remote Gui（下文简称TRG），可在windows下安装，界面友好且信息直观。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-e26e5c21fb181cda63ff244990ad04e6_720w.jpg)

Transmission 的WeiUI



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-1009ee874cd46f119ab986ce93da51a4_720w.jpg)

TRG的界面

## 2、Docker内安装Transmission

大部分的安装过程与Aria2一样，所以讲解简化。

首先我选的镜像是linuxserver/transmission，可以卡到这个镜像的人气很高，240颗星。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-40b73bd5599863e0ee109a9294d05f93_720w.jpg)



等待下载的时间里可以来看看配置说明。-v的三个文件夹分别对应的是配置文件、下载文件、监控文件，监控文件夹是用于自动下载放进来的种子文件的，类似群晖自带下载器里的功能。这个文件夹不设置也行，会默认放在下载文件夹下方。-e三个环境值分别对应用户名、密码、和时区，其中用户名和密码需要填写群晖中的，用于获取相应的磁盘权限，时区这个实测不需要填，我也不懂怎么填。-p的端口9091是UI端口，51413是默认的BT下载端口。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-ae39fb49fc16c44b93e89e09c91a3768_720w.jpg)



安装后启动进入高级设置，按上面的配置说明填好各个标签页。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-d131bb5750c6497faaa9d7d9548c70f3_720w.jpg)





![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-20e43b01fcada853939a034dbf3dee7b_720w.jpg)





![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-473ac164ee673cdb434c916981747f1e_720w.jpg)

后面4个是默认的环境变量，不管他

应用并启动之后，Transmission就开始运行了，打开TRG，点击图中箭头所指的图标选择新的连接，设置好名称和群晖的IP及端口，就能连上下载服务器了。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-20d39a000fb11ef6d4b23c54ee1ae09f_720w.jpg)



此时点击界面上的相应按钮就已经可以开始下载了。图中红箭头命令为打开种子文件，蓝箭头命令为打开种子链接或磁力链接。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-7533f284a59754db2984d02ea98f7b94_720w.jpg)



## 3、Transmission的配置

连上服务器后按F9或者点击图中箭头所指的图标，即可进入Transmission的全局设置。大多数设置默认即可，主要关注第二页中红框内的三个选项，与Aria2中一样，如果使用PT下载，则全都不要勾选，如果选择BT下载，就都勾上吧。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-c18f47cd2b22c8b800de293301316c83_720w.jpg)





![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-9e7af57e276d078815dae4bfef32b2ce_720w.jpg)



也可以像Aria2一样通过配置文件进行设置，不过其实UI界面内的设置已经足够普通玩家使用了。如果想要更全面的对服务器进行设置，可以往下看。启动Transmission后1config文件夹中会自动生成Transmission的配置文件，文件名称为setting.json。找到config文件夹中的双击下载后用写字板打开，就能对其进行编辑了。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-d3cadd6935e022bb8fbd1513e81a5021_720w.jpg)



Transmission的配置命令比Aria2少蛮多，但看着仍然头大。好在网上也有人将所有的参数对应的解释汇总放出来了，可以根据自己的需要对照着进行修改。 **（具体内容请戳原文。）**

Transmission中的incomplete文件夹是用于存放未下载完成的文件，watch文件夹用于监视种子文件，个人觉得都没有必要，可以在配置中改成false。同样需要注意根据自己BT或PT的需求修改DHT/LPD等选项是否开启。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-08cfbe7f3134936168f1f8e11a4f3479_720w.jpg)



配置文件修改完后存回config文件夹并覆盖原文件，这时重启一次Transmission，就可以按新的配置来运行了。

## 四、配合DDNS和端口转发实现远程下载

上一篇中已经通过公网ip+DDNS实现了外网访问，下载器设置好之后，同样可以通过端口转发来实现外网访问，从而实现远程下载。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-411dc88841562a719acd0fe6c5f8a64c_720w.jpg)



其中6690为Drive实现外网访问的端口，上一篇提交时还没有发现Drive有特殊端口，所以这里补充一下。在Drive中将连接设为DDNS:6690即可实现外网访问NAS并同步文件。这样出门在外，重要资料随时备份，妈妈再也不用担心我弄丢工作资料了。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-3e9f439f4db98a2741349d2f486e6ba0_720w.jpg)





![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-333b02740e412482dc00e3982e0efeb5_720w.jpg)

Aria2远程连接



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-ea6183346070d8f0a292eaf2c0835486_720w.jpg)

Transmission远程连接

另外推荐一个手机APP，仅限安卓手机，名字叫做Transdrone，可能需要从谷歌商店下载，或者自行寻找apk吧。这个软件既可以连接Transmission服务器，又可以连接Aria2服务器，通杀两个下载器真心超方便。可以随时使用安卓手机监控下载情况，也能通过种子文件或链接的形式添加下载任务，从此指尖操作告别电脑端。



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-a18bc40b15debd3048b2681cbb6ce102_720w.jpg)

此处借用谷歌商店官方图

其实以前ios也有一个类似的App叫做Monitor，同样可以实现对下载器的监控，可是目前已经不再提供下载了，我之前删除了现在想下也下不到了。

不知道ios现在有没有替代APP，希望有大神能够分享一下。用老婆的手机截图分享一下吧



![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-951bbc8bf93ed1373ac11160d5558bc7_720w.jpg)





![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-28e24d3b2962320b1529624002cc5a44_720w.jpg)



## **总结**

从一年多前接触PT，开始学着用Transmission和Aria2，爬过一个又一个的坑。以前在梅林路由器和zidoo网络盒子上玩，都是OpenWrt系统，找到的教程要不就是只教你安装，要不就是只教你配置，可是偏偏安装和配置的教程又大多不匹配，设置文档的路径什么的不一样，再加上我没用过Linux，写代码更是一窍不通，导致平添了很多的障碍。在群晖的docker中安装这些软件就很方便了，一键式安装包，窗口化界面的安装设置，让整个安装配置过程顺畅了。也正因为此，我才能写出这样一篇从安装到配置一站式的分享教程。

技术类长文内容枯燥，感谢看完的各位~下一篇会对实现家庭媒体中心功能的Emby+Kodi+ios播放器进行分享，敬请期待。



by：Mr_JF

**本内容来源于@什么值得买[http://SMZDM.COM](https://link.zhihu.com/?target=http%3A//SMZDM.COM)**

**原文链接：**[新司机的黑裙战斗机 篇五：【进阶】玩转群晖中好用的下载工具们——Aria2+Transmission__什么值得买](https://link.zhihu.com/?target=https%3A//post.smzdm.com/p/a78eq7v5/)

![img](/img-post/开发/NAS/玩转群晖中好用的下载工具——Aria2+Transmission.assets/v2-d86c3357518cff4b993cfce06f489ec1_720w.jpg)

发布于 2018-11-19