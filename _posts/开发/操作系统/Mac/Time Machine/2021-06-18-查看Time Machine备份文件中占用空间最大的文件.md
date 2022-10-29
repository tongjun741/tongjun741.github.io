---
tags:
    - 操作系统
    - Mac
    - Time Machine
---

```
# 比较两次备份的差异并输出到文件
tmutil compare -s '/Volumes/Time Machine/Backups.backupdb/tongjun’s iMac/2021-06-17-114359' '/Volumes/Time Machine/Backups.backupdb/tongjun’s iMac/2021-06-16-171851'  > ~/Desktop/compare.txt

# 比较两次备份的差异并按修改大小排序
 tmutil compare -s '/Volumes/Time Machine/Backups.backupdb/tongjun’s iMac/2021-06-17-114359' '/Volumes/Time Machine/Backups.backupdb/tongjun’s iMac/2021-06-16-171851'  | sort -h -k 2
 
 # 比较两次备份的差异并按修改大小排序，只查看按添加项（+）和修改（!），不看删除（-）
 tmutil compare -s '/Volumes/Time Machine/Backups.backupdb/tongjun’s iMac/2021-06-17-114359' '/Volumes/Time Machine/Backups.backupdb/tongjun’s iMac/2021-06-16-171851' | grep '^[\!+]' | sort -h -k 2
```



https://blog.51cto.com/liongmagezi/1358382

https://cn.bornwithaspergers.com/930717-sort-output-of-tmutil-compare-UOTHYF