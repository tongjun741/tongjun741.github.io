---
tags:
    - 操作系统
    - Linux
    - 查找文件
---

linux – rsync – 排除超过一定大小的文件



```javascript
有一个max-size选项：
--max-size=SIZE         don't transfer any file larger than SIZE
所以：
# rsync -rv --max-size=1.5m root@tss01:/tmp/dm
将仅发送小于1.5米的文件.

```





https://codeday.me/bug/20181105/356086.html

