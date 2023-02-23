---
tags:
    - NAS
---

```javascript
   群晖moments的手机照片、视频同步带给人极大的方便，但是囊中羞涩买不起正版的群晖的而玩黑群晖的必定经历过Moments视频不显示缩略图的苦恼，小编搜遍全网，比对发现安装第三方FFmpeg是最快的解决方法。

  
 
```

![在这里插入图片描述](/img-post/开发/NAS/黑群晖Moments视频无缩略图，安装第三方ffmpeg解决.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTQ2NDg2,size_16,color_FFFFFF,t_70-20210627230425487.png)

操作步骤：

开启SSH

控制面板——终端机和SNMP——勾选启动SSH功能——应用
![在这里插入图片描述](/img-post/开发/NAS/黑群晖Moments视频无缩略图，安装第三方ffmpeg解决.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTQ2NDg2,size_16,color_FFFFFF,t_70-20210627230425635.png)

添加第三方源

套件中心——设置——常规——选择任何发行者

套件中心——设置——套件来源——新增
![在这里插入图片描述](/img-post/开发/NAS/黑群晖Moments视频无缩略图，安装第三方ffmpeg解决.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTQ2NDg2,size_16,color_FFFFFF,t_70-20210627230425818.jpeg)
![在这里插入图片描述](/img-post/开发/NAS/黑群晖Moments视频无缩略图，安装第三方ffmpeg解决.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTQ2NDg2,size_16,color_FFFFFF,t_70.png)

名称：随意

位置：

http://packages.synocommunity.com

安装第三方ffmpeg

套件中心——社群——安装ffmpeg
![在这里插入图片描述](/img-post/开发/NAS/黑群晖Moments视频无缩略图，安装第三方ffmpeg解决.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTQ2NDg2,size_16,color_FFFFFF,t_70-20210627230425818.png)

备份原始ffmpeg、创建第三方ffmpeg软连接

1、ssh连接群晖（利用putty、xshell等），登录后切换root账号

sudo -i

提示输入密码时输入当前用户密码
![在这里插入图片描述](/img-post/开发/NAS/黑群晖Moments视频无缩略图，安装第三方ffmpeg解决.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTQ2NDg2,size_16,color_FFFFFF,t_70.jpeg)

2、备份原始mmfpeg

mv /usr/bin/ffmpeg /usr/bin/ffmpeg_BAK
![在这里插入图片描述](/img-post/开发/NAS/黑群晖Moments视频无缩略图，安装第三方ffmpeg解决.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTQ2NDg2,size_16,color_FFFFFF,t_70-20210627230425617.png)

3、创建第三方ffmpeg软连接

ln -s /volume1/@appstore/ffmpeg/bin/ffmpeg /usr/bin/ffmpeg

进入Moments套件，设置——常规——重建索引

稍等片刻（具体时间看Moments中照片视频数量），刷新，搞定！

![在这里插入图片描述](/img-post/开发/NAS/黑群晖Moments视频无缩略图，安装第三方ffmpeg解决.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTQ2NDg2,size_16,color_FFFFFF,t_70-20210627230425712.png)

文章来源: blog.csdn.net，作者：木马的翅膀，版权归原作者所有，如需转载，请联系作者。

https://blog.csdn.net/qq_33546486/article/details/112243135