---
tags:
    - 操作系统
    - Linux
    - 查找文件
---

当用find和xargs来处理文件时，如果文件名包含空格，会导致处理失败。

在find的帮助中，有一个参数-print0:

-print0
              True; print the full file name on the standard output, followed by a null character (instead of the newline character that -print uses).  This allows file names that con‐
              tain newlines or other types of white space to be correctly interpreted by programs that process the find output.  This option corresponds to the -0 option of xargs.

当用上这个参数时，如果文件名包含有空格类型的字符时，会用null字符来替换，从而不会被解释为换行符
而在xargs的帮助中，有-0这个参数：

-0, --null
              Input items are terminated by a null character instead of by whitespace, and the quotes and backslash are not special (every character is taken literally).  Disables  the
              end  of  file string, which is treated like any other argument.  Useful when input items might contain white space, quote marks, or backslashes.  The GNU find -print0 op‐
              tion produces input suitable for this mode.

这里最后有提到，是为了匹配像find -print0这样的模式，把null字符重新解释为空格
到了这里，我想大家就明白了，来个例子：

```
[root@localhost temp]# find . -type f -name test* -print0 | xargs -0 md5sum
e74a5cebda37864d767b48a6052ca2d8  ./test. php
[root@localhost temp]# md5sum ./test.\ php
e74a5cebda37864d767b48a6052ca2d8  ./test. php
```



————————————————
版权声明：本文为CSDN博主「云中不知人」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u011085172/article/details/77771173