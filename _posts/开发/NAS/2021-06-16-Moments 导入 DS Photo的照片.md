---
tags:
    - NAS
---

# 群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！

https://post.smzdm.com/p/av7zn43p/

2020-02-18 09:39:53 501点赞 4826收藏 582评论

**创作立场声明：**群晖的Moments是非常优秀的相册管理软件，这次我来分享一下更好用的小技巧，轻松管理10万+照片！

## 前言

大家好，俺又来了！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

现在很多朋友玩unraid系统，而大多数朋友，最希望能有一个非常好用的**[相册](https://www.smzdm.com/fenlei/xiangce/)管理软件！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/38.png)**

比如[群晖](https://pinpai.smzdm.com/2315/)的Moments，不知道Moments的朋友可以看看下面本站 @bill_bill的文章：

[![img](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5c9f70d5e07a26451.jpg_a200.jpg)](https://post.smzdm.com/p/adwlmzvd)[**群晖NAS使用记：就是这个时刻，就是这个Moments**](https://post.smzdm.com/p/adwlmzvd)两年前在值得买写过一篇文章《老笔记本也当NAS，自动备份全家手机照片》，介绍了一个windows下的自动手机图片、视频备份软件DAEMONSNYC。虽然很实用，但是不知道是不是软件不够成熟，在实际使用的时候总会有点小麻烦，就是只能同步一部分就暂停/或者退出了，这让人很恼火。今天要介绍的这个软件，就是[bill_bill](https://zhiyou.smzdm.com/member/4123143329/)|*赞*82*评论*88*收藏*538[查看详情](https://post.smzdm.com/p/adwlmzvd)

**我上篇用虚拟机直通的方式**，在unraid下使用群晖NAS系统，这样就可以完美的使用群晖和相册软件了。

上篇文章：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

[![img](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e49766d326bb3394.jpg_a200.jpg)](https://post.smzdm.com/p/awx0nglg)[**家用媒体服务器NAS 使用UNRAID系统的正确的玩法！直通网卡、直通硬盘、挂载群晖虚拟机文件！**](https://post.smzdm.com/p/awx0nglg)蜗居的阿宅终于有了正当理由不出门，那么他们的生存秘诀是什么？全新上线的#宅家生活手册#征稿活动火热进行中，来分享你特殊时期的宅居规划吧>活动详情戳这里<时间：1月28日-2月29日前言大家好，俺又来了！本文内容有点长，而且十分干货，建议先收藏，再观看！最近一直忙着出威联通相关的折腾文章，忽略了unr[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*452*评论*484*收藏*2k[查看详情](https://post.smzdm.com/p/awx0nglg)



**要是您真的非常不想用虚拟机来管理照片，还可以用Docker软件来实现，效果要差一些。**

这里推荐 荒野无灯大佬制作荔枝相册 和 PLEX相册，之前都出过文章：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/38.png)

[![img](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5d2027fef00c87017.jpg_a200.jpg)](https://post.smzdm.com/p/apze0pl7)[**使用群晖Docker功能 三分钟安装轻量的相册程序 lychee 荔枝相册**](https://post.smzdm.com/p/apze0pl7)大家好，俺又来了，这次大家分享群晖Docker安装非常轻量的lychee荔枝相册的方法！这次安装的lychee荔枝相册是灯大（荒野无灯）在管理，优化已经非常满意了，不仅可以公开展示自己的照片，还可以展示视频。而且打开速度非常的快，个人觉得比群晖自带的DSphoto要更适合展示照片。比如俺的小站里的相[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*98*评论*121*收藏*780[查看详情](https://post.smzdm.com/p/apze0pl7)



[![img](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e3fb82f3eedc8878.jpg_a200.jpg)](https://post.smzdm.com/p/amm5rmrp)[**界面漂亮，全平台通用：300元购买 PLEX Pass会员！打造私人 家庭影院 媒体服务器！**](https://post.smzdm.com/p/amm5rmrp)蜗居的阿宅终于有了正当理由不出门，那么他们的生存秘诀是什么？全新上线的#宅家生活手册#征稿活动火热进行中，来分享你特殊时期的宅居规划吧>活动详情戳这里<时间：1月28日-2月29日大家好，俺又来了！今天给大家介绍一款家用的影音媒体管理器，PLEX！PLEX和Jellyfin很像，都是将NAS或者电脑[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*478*评论*574*收藏*4k[查看详情](https://post.smzdm.com/p/amm5rmrp)

最后，我还是**会推荐群晖的Moments相册软件**，真的非常的不错！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

## 为什么出这篇文章?

1、肯定有值友会说，为什么会出这篇文章，群晖相册软件我们都在用呀，有什么问题么？![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/40.png)

起因是，今天在群聊中，**很****多朋友说群晖的Moments特别的好用！**![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/38.png)

都在问unraid下有没有什么软件推荐！我的答复是，只有群晖的Moments，或者[威联通](https://pinpai.smzdm.com/3063/)的Qumagie相册，其它的都不太好用。

目前我的威联通和群晖NAS都管理了10万多张照片，**家里4-5个人手机都能自动同步备份**，只手机照片就有370G，但是威联通只有买白的，没法很好的黑威：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/40.png)

**威联通管理了11万张照片：**

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a93d15b4712953.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_2/)

照片占用370G：

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a93f89cb777693.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_3/)

2、威联通的Qumagie相册和群晖的Moments 功能都非常的相似，这2个软件都非常的好用，人脸识别，场景识别，AI，都很完美：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/39.png)

相册软件：

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a940cc160d5433.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_4/)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a940d0db429377.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_5/)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a940d0fe422901.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_6/)

3、**但是本文****，不是来吹威联通多么强的**，而是告诉大家，**群晖下Moments的正确用法，因为很多人搞错了**：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/27.png)

很多用群晖Moments的朋友，**都****是只用这一个软件，这是非常不推荐的！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/58.png)**

因为Moments会产生很多以为照片日期来新建[文件夹](https://www.smzdm.com/fenlei/wenjianjia/)进行分类，这样非常不方便管理：

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a943537ecc1524.png_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_7/)

4、特别是使用的人数一旦多起来了，拍摄的设备再一多，这种方式就特别的混乱！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/40.png)

Moments是好用，但是这种备份照片的方式，真的很乱：

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a9474b0f756280.png_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_8/)

5、而我推荐的解决方案：是使用Photo Station来进行备份！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/64.png)

这样就可以手动设置相册的名字，手动设置自动备份的文件夹目录，再用moments来进行管理和AI识别：

**Photo Station的相册文件夹：**

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a95c7e6c777054.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_9/)

6、我们只需要在Moments里面设置一下共享照片库，就可以识别Photo Station里面的照片了，而且一样支持AI场景、人脸识别：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a94b425a311662.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_10/)

7、我的群晖NAS中，也同时管理着自己的10万张照片！我用群晖很多年了，一直都是忠实的用户，这个功能确实吹爆：

大家用群晖Moments的照片，估计都没我的多吧？照片越多，识别的内容也非常的多，真的锤爆这个功能：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/22.png)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a94d70a12b1720.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_11/)

下面详细演示下操作流程！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

## 群晖NAS相册的正确玩法

1、首先，我们需要在群晖套件中心里安装 2个相册：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

**Moments：**

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a965e5bbc96654.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_12/)

**Photo Station：**

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a965e5e57b3772.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_13/)

2、用DSphoto 手机APP进行同步备份手机的照片，设置好相册目录，这个目录就在群晖的photo文件夹下：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/38.png)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a9e3a316787214.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_14/)

3、也可以将相机拍摄的照片，整理好后，复制到群晖photo文件夹下面的文件夹，并且可以自己定义的日期+项目的方式来命名存放：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

Photo Station 也会自动对照片进行转码，生成略缩图，加快浏览体验：

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a9756e651f7768.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_15/)

4、然后，我们打开Moments软件，点击左下角的设置按钮：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/22.png)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a9780c76952208.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_16/)

5、选择启用共享照片库，这个必须要配合Photo Station才好玩：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/43.png)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a97c06b8bf6.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_17/)

6、点击最下面的全部重建索引：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/67.png)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a98059e10c8936.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_18/)

7、等一阵子后，在左上角的Moments这里就可以选择共享文件夹：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/109.png)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a983a50fa73974.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_19/)

8、这样，我们的照片，就可以非常好的管理和分类了，这就是正确的玩法：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/24.png)

**群晖Moments 识别 Photo Station：**

我的照片数量是非常的多的，识别后，进行观赏也很有意思：

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a9850cf72e7628.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_20/)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a9852761a01622.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_21/)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a9851649c32956.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_22/)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a98520098f3578.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_23/)

**吹爆群晖相册这个功能！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)**

## 最后总结

**我们简单屡一下：**

**1、用Photo Station 进行管理和备份照片，家里全家人的手机都安装DS Photo APP来进行备份，设置一个相册目录在photo文件夹中。**

**2、自己相机拍摄的照片，整理导出jpg格式后，直接放到photo对应的相册文件夹内：**

**3、在Moments里面开启共享照片库，点击索引，这样就完成了！**

**最后聊一下我的存放照片设备：**

1、我的设备，威联通是24小时不关机的，来实时备份家人的手机照片，2块硬盘，组的raid1！威联通的Qumagic软件能识别自定义的文件夹，这点还是很方便的，不需要像群晖一样搭配起来使用：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/22.png)

威联通官方对黑威联通有打击行为，而且几乎半个月就要更新一次系统，想用好的相册软件：

要么黑群晖，要么白群晖，要么白威联通了，没有其它的选择了！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/36.png)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a990d854252404.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_24/)

2、我的群晖NAS，是一台比较高性能的8盘位万兆NAS：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/39.png)

配置是E3处理器，C236[服务器](https://www.smzdm.com/fenlei/fuwuqi/)主板，ECC内存，万兆网卡，就是参考B站翼王的万兆NAS搭建的，整机下来不含硬盘至少4000+，硬盘塞了5块8T、3块10T：

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a994dc030a781.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_25/)

3、这台安装群晖NAS系统的机器，因为功耗比较多，所以平时并不会经常开机，当需要工作的时候才开机，比如它可以流畅的进行在线剪辑，在线修图，素材都放NAS里：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

一般1-2个星期左右，我会将威联通备份的照片文件夹，再复制到群晖photo文件夹中，这样不需要再用手机上传，群晖NAS的Moments都可以识别：

这样一来，比 Moments 和 DS Photo的上传，还要方便：![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/39.png)

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a99bfdfb7c4673.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_26/)

**其实，群晖和威联通这2个相册软件功能是一样的，都是AI识别，人脸识别，场景识别，但是两家的体验却略不一样，总之，每次看到的照片，都是非常新奇的**！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

**比如下面同样识别天空场景，两家的AI识别的就有些区别：**

**威联通：**

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a9a6959b9b5949.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_27/)

**群晖：**

[![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/5e4a9a695f52a4158.jpg_e1080.jpg)](https://post.smzdm.com/p/av7zn43p/pic_28/)

**相册软件，这种东西，是越用得久，就越离不开的！**

**而用得越多，越有意思！我妈上次就惊叹，她用手机拍的娃儿照片，怎么在我的电脑上也有？![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/24.png) 我点了人脸识别，就可以找到5部手机，N个相机的同一个人的照片了！**

**特别是我10年来的照片都放进去用这个进行管理，选择人脸识别，时间线模式，可以明显看到10年来大家的变化，真的是非常非常棒，再也离不开了，这个AI识别真的牛！！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/38.png)**

目前接触的NAS的用户，80%使用NAS都是因为**有优秀的相册软件**！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/43.png)

照片是非常珍贵，也感谢群晖和威联通能制作这么优秀的相册管理软件。据说群晖在DSM7.0以后，相册软件还会大升级，非常的期待！！大家用群晖NAS系统用得爽的朋友，希望能转正哦！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

**好了，这就是本文的全部内容了，感谢您的观看，如果觉得文章写得还不错的话，欢迎点赞、收藏、评论一条龙。**

我现在**威联通、群晖、unraid**都在使用，平时使用中遇到的的经验，会乐于分享给大家。

如果说的不对的地方，也希望大家轻喷，可以留言区补充，也欢迎大家多多点赞，点赞用户有问必答！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/33.png)

我们下次再见，白白！下次见！白白！![群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/39.png)

![img](/img-post/开发/NAS/Moments 导入 DS Photo的照片.assets/the-end.png)