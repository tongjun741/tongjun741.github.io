---
tags:
    - 操作系统
    - Mac
    - Time Machine
---

删除Time Machine中备份的旧系统



```javascript
先是tmutil machinedirectory 得到备份的目录列表

/path_to_backup

再用 sudo tmutil delete /path_to_backup 即可删除指定的备份

关于MAC 的TimeMechine备份，如何手动清除备份占用的空间? - 倪焱石的回答 - 知乎
https://www.zhihu.com/question/47951339/answer/790487489

```





另外参考：

```javascript
sudo /System/Library/Extensions/TMSafetyNet.kext/Contents/Helpers/bypass rm -rf 2019-06*

```



https://superuser.com/questions/162690/how-can-i-delete-time-machine-files-using-the-commandline





