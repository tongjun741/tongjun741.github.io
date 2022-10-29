---
tags:
    - NAS
---



**追加修改(2020-11-14 02:29:20):**
补充一句： 如果在DSM的资源监控→任务管理器里，看到Synology Drive Server不断地长时间地读写，而PC端显然是没有在传输、同步文件的 ——那么，这也是Synology Drive在做团队文件夹的备份 ——所以，不需要使用Synology Drive同步的团队文件夹，直接停用就好了



## 教程背景

因为真正需要多重备份的文件不多，所以我[群晖](https://pinpai.smzdm.com/2315/)没有组RAID，直接basic

重要文件存了一份在[硬盘](https://www.smzdm.com/fenlei/yingpan/)A，然后使用Hyper Backup把它们再备份到硬盘B，这样重要文件就在2个硬盘存了2份

另外，重要文件我还使用Synology Drive实时同步到了PC，这样就存在3个硬盘上，足够放心

最近发现一个问题：Hyper Backup的备份任务，竟然搞出了近3T+的大小，本来充裕的硬盘一下被塞♂满了

需要备份的源文件一共就500G左右，备份却搞出了5、6倍+的占用空间

一开始我还以为是Hyper Backup出了问题

尝试了：删除备份任务、文件，再次备份，结果备份出来的文件还是很♂大

尝试了：减小Hyper Backup任务的轮换版本数量，但是空间还是没有回来

[![群晖Hyper Backup备份的文件很大，处理方法的设置教程](/img-post/开发/NAS/群晖Hyper Backup备份的文件很大，处理方法的设置教程.assets/5fae86e7164242515.jpg_e1080.jpg)](https://post.smzdm.com/p/a07nzdgw/pic_2/)

后来仔细看了一下，在Hyper Backup这里：这类[文件夹](https://www.smzdm.com/fenlei/wenjianjia/)明明没勾选的，但是它也出现在Hyper Backup的备份文件里

——才知道原来是因为Synology Drive：它备份的文件，同样会体现在Hyper Backup里

也就是说，Hyper Backup显示的文件大小，也包括了Synology Drive备份文件

那么解决“Hyper Backup备份生成的文件很大”的问题，方法就 呼♂之♂欲♂出 了，需要做2个设置：

优化Synology Drive的设置、删除Hyper Backup的版本

这下多占用的空间就会回来了

## 第1步：优化Synology Drive的设置 

[![群晖Hyper Backup备份的文件很大，处理方法的设置教程](/img-post/开发/NAS/群晖Hyper Backup备份的文件很大，处理方法的设置教程.assets/5fae8888460fd2118.png_e1080.jpg)](https://post.smzdm.com/p/a07nzdgw/pic_3/)

**（1）【停用】不需要的团队文件夹**

——这是给我带来问题的根源：有个video文件夹，我存了2T的电影文件，手贱启用了团队文件夹，并且设置了版本数量5

实际上这个文件夹完全不需要备份的，这下却都“存到”了Hyper Backup，害

总之，不需要使用Synology Drive同步、备份的文件夹，直接在团队文件夹里停用掉就好了

**（2）【减小】其它团队文件夹的版本数量**

——这个就不多解释了，根据自己需求调整就好。

备份的版本数量越多，在Hyper Backup里“占用”的空间就越大

通过以上2步，操作完成后，Synology Drive会把多余的版本文件删掉

但是空间还没有完全回来，因为最大坨的文件还在Hyper Backup里，所以需要：

## 第2步：删除Hyper Backup的版本 

[![群晖Hyper Backup备份的文件很大，处理方法的设置教程](/img-post/开发/NAS/群晖Hyper Backup备份的文件很大，处理方法的设置教程.assets/5fae87d462fde7988.jpg_e1080.jpg)](https://post.smzdm.com/p/a07nzdgw/pic_4/)

把不需要的版本删除掉，之后空间就会回来了

——主要是要需要删除包含Synology Drive生成的不必要的备份的版本

——当然，你也可以马上跑一次备份任务，生成一个最新的版本（任务完成备份不会很久），然后把以前的都删掉

我这里只有显示一个版本，是因为已经删除过了

由于之前生成的备份文件很大坨，所以Hyper Backup的删除也要蛮久的，但是没关系，It will be back



https://post.smzdm.com/p/a07nzdgw/