---
tags:
    - 操作系统
    - Linux
    - 查找文件
---

Linux下使用find和grep查找不包含某些字符串的文件



```javascript
find ./ -name "*.html" |xargs grep -L "recordRefid"

```



