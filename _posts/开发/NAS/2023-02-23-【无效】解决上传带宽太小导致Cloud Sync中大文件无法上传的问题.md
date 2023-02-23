---
tags:
    - NAS
---

### 此方案无效

NAS中有许多大文件要通过Cloud Sync上传到网盘，但家里的上传速度不到3MB/s，而Cloud Sync最小只能设置3个任务同时进行，这就导致Cloud Sync会一直卡在上传3个文件中，而且由于上传时间才长导致上传失败再重试，这3个大文件一直上传不成功。

目前的对策就是修改同时传输的数量为1，这样就能让一个大文件利用全部的上传带宽，降低超时的概率。

具体操作是通过模拟请求，把提交的数据中传输的数量的值由3改为1，无需重启Cloud Sync。

![image-20220311113103018](/img-post/开发/NAS/解决上传带宽太小导致Cloud Sync中大文件无法上传的问题.assets/image-20220311113103018.png)





https://community.synology.com/enu/forum/17/post/91955