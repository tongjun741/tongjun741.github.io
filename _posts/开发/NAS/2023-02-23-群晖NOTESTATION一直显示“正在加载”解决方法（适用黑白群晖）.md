---
tags:
    - NAS
---

一、问题原因
最近套件中心提示NoteStation要升级（版本 2.6.0-1407），于是手贱点了升级按钮。

升级完成后打开NoteStation一直显示 “正在加载” ，等待许久也没有结束，还好PC客户端和手机客户端是正常的。

![正在加载](/img-post/开发/NAS/群晖NOTESTATION一直显示“正在加载”解决方法（适用黑白群晖）.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nemVqaW4zODgz,size_16,color_FFFFFF,t_70-20210610094124479.png)

显然是升级新版的NoteStation导致的，于是重新安装旧版本的NoteStation解决问题。

二、解决办法
1、备份数据
首先备份数据（虽然没有用到备份，但是以防万一先备份吧），打开NoteStation，不用理会正在加载，点击顶部设置按钮，选择导入和导出选项卡，点击导出按钮，随便选择一个地方即可。

![NoteStation备份数据](/img-post/开发/NAS/群晖NOTESTATION一直显示“正在加载”解决方法（适用黑白群晖）.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nemVqaW4zODgz,size_16,color_FFFFFF,t_70.png)

2、卸载NoteStation
打开套件中心，找到NoteStation，在动作下拉按钮中选择卸载，一路下一步，注意不要勾选下图选项，最终点击应用按钮等待卸载完成。

![卸载NoteStation](/img-post/开发/NAS/群晖NOTESTATION一直显示“正在加载”解决方法（适用黑白群晖）.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3poYW5nemVqaW4zODgz,size_16,color_FFFFFF,t_70-20210610094124487.png)

3、安装旧版本NoteStation
旧版本NoteStation下载路径如下==（需要下载x86版本 NoteStation-x86_64-2.5.5-0870.spk）==：

https://archive.synology.com/download/Package/NoteStation/2.5.5-0870

> 链接：https://pan.baidu.com/s/1hLggiO96hXCC6AZX1jEx7A
> 提取码：note
> 复制这段内容后打开百度网盘手机App，操作更方便哦–来自百度网盘超级会员V4的分享

下载完成后记得解压缩

打开套件中心点击顶部的手动安装按钮，然后选择刚刚下载并解压好的文件，然后一路下一步，等待完成安装。

安装完成后记得刷新浏览器，然后打开NoteStation秒加载完成，彻底解决NoteStation一直显示 “正在加载” 的问题。

另外再提示NoteStation更新请暂时不要理会了，等过些时日再尝试更新吧。

三、总结
没想到群晖官方这么不靠谱，没有测试通过就发出来更新，以后再更新要谨慎些…
————————————————
版权声明：本文为CSDN博主「小歆Pro」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zhangzejin3883/article/details/113782469