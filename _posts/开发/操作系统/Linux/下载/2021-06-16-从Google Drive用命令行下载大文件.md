---
tags:
    - 操作系统
    - Linux
    - 下载
---

从Google Drive用命令行下载大文件

# 前言

很偶然，实验室伙伴的小伙伴找到我帮忙下外网数据集，然后我帮着忙，顺便发现了一个简便的方法从云盘下文件…

原来的方法来自于[Quora](https://www.quora.com/How-do-I-download-a-very-large-file-from-Google-Drive/answer/Shane-F-Carr)，英语没问题的话看原帖就好了。  
顺便，如果实在无法翻墙的同学要下数据集的时候，可以购买国外公有云服务器，先下载到云服务器上，再从云服务器上搬运回来。

# 全步骤

以下是使用命令行API从Google Drive上下载文件的详细步骤，前提是文件是私有分享并且需要身份认证的。

## 获取文件ID

1.  登录Google云盘（最近跟梯子有关的帖子都被屏蔽了欸）；
2.  右键点击（或者直接点击）要下载的文件，选择“获取分享链接”。链接的形式为`https://drive.google.com/open?id=XXXXX`，其中的`XXXXX`就是下面会用到的文件ID。

![获取可分享链接](https://img-blog.csdnimg.cn/20190310130923580.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1Y2ljaGV1bmc=,size_16,color_FFFFFF,t_70)

## 获取OAuth token

1.进入[OAuth 2.0 Playground](https://developers.google.com/oauthplayground/)；  
2\. 在`Step 1 Select & authorize APIs`滚动框中，选择`Drive API V3==`，并且选中`https://www.googleapis.com/auth/drive.readonly`;  
3\. 点击按钮`Authorize APIs`之后选中`Exchange authorization code for tokens`，获得`Access token`，对`Access token`进行复制供后续步骤使用。

## 从命令行下载文件

注意：下列命令行中，用文件ID替换`XXXXX`，用`Access token`替换`YYYYY`，用保存文件名（含后缀，如"myfile.zip"）替代`ZZZZZ`。

### 类Unix系统

打开终端，输入以下命令：

```
curl -H "Authorization: Bearer YYYYY" https://www.googleapis.com/drive/v3/files/XXXXX?alt=media -o ZZZZZ.iso

wget --header="Authorization: Bearer YYYYY"  https://www.googleapis.com/drive/v3/files/1tCqH1rkw9YXOs--UXcY5RmsE_RRuXJYx?alt=media -O "macOS Mojave 10.14 by SYSNETTECH Solutions Full Version.iso"

```


### windows系统

打开powershell（不知道在哪里的话，用Cortana搜索下就好）,输入以下命令 ：

```
Invoke-RestMethod -Uri https://www.googleapis.com/drive/v3/files/XXXXX?alt=media -Method Get -Headers @{"Authorization"="Bearer YYYYY"} -OutFile ZZZZZ

```
https://blog.csdn.net/yucicheung/article/details/88374064