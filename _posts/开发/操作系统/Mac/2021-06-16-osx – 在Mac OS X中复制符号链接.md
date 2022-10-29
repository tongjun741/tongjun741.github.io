---
tags:
    - 操作系统
    - Mac
---

osx – 在Mac OS X中复制符号链接

正如David所说，OS X缺少gnu cp的方便的选项。

但是，如果使用-R来执行递归副本，那么默认情况下会复制符号链接

```javascript
cp -R source destination

```

应该工作

https://codeday.me/bug/20180420/154940.html

