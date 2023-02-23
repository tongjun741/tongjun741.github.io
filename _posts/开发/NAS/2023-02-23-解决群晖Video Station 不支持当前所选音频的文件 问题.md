---
tags:
    - NAS
---

## 如果失效需要重启Video Station套件



由于群晖新版Video Station套件没有 DTS 和 eac3授权，在播放一些高清电影时会提示“不支持当前所选音频的文件格式，因此无法播放视频。请尝试其它音轨。。。”
以下是解决方法，此方法无需使用旧版Video Station套件，直接用新版套件即可。

[![Video Station](/img-post/开发/NAS/解决群晖Video Station 不支持当前所选音频的文件 问题.assets/4035640510.png)](https://ioctop.com/usr/uploads/2021/05/4035640510.png)Video Station



## 1、首先安装ffmpeg解码器

去群晖套件中心里面的设置添加源
[http://packages.synocommunity.com](http://packages.synocommunity.com/)
添加后会在左边套件分类列表中多出来一个<<社群>>分类
在社群内找到FFmpeg然后安装它

## 2、安装最新版Video Station套件

适用于最新2.4.10-1632版 Video Station

首先SSH登录，切换root 用户下：

```
sudo -i
```

执行下列一键安装脚本：

```
sh -c "$(wget -O- https://raw.githubusercontent.com/crazykuma/dsm_plugins/master/ffmpeg_dts_eac3_patch.sh)" -p install
```

安装后重新启动 Video Station 就可以播放 `DTS 和 eac3` 音轨的视频了，`硬件解码`也正常，完美！

卸载补丁执行下列命令：

```
sh -c "$(wget -O- https://raw.githubusercontent.com/crazykuma/dsm_plugins/master/ffmpeg_dts_eac3_patch.sh)" -p uninstall
```

https://ioctop.com/archives/241.html