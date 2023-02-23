---
tags:
    - IDE
    - VS Code
---

如果安装插件，下面的内容其实都应该是可以实现的。

如果不安装插件，下面的内容在实现上进行了一些探讨：

打开File->Preferences->Settings的Text Editor->Files的Associations的Edit in settings.json选项，会打开settings.json配置文件。
添加下面内容，后缀名为.myCustomFileSuffix的自定义文件会按照javascript文件的语法格式显示语法高亮，并格式化文档:
C:\Users\<user name>\AppData\Roaming\Code\User\settings.json

```
{
    "files.associations": {
        "*.myCustomFileSuffix": "javascript",
    },
    "javascript.format.placeOpenBraceOnNewLineForFunctions": true,
    "javascript.format.placeOpenBraceOnNewLineForControlBlocks": true
}
```



定义Javascript的左大（花）括号后是否放置在新的一行：
设置File->Preferences->Settings的Extensions->TypeScript的选项：
JavaScript>Format: Place Open Brace On New Line For Control Blocks
Javascript>Format: Place Open Brace On New Line For Functions
这2个选项的修改会记录在settings.json配置文件中。

坦白讲，“左大（花）括号后是否放置在新的一行”也没有那么重要，如果VSCode默认标准如此，也不是一定要修改。

打开File->Preferences->Settings的Text Editor->Files的Auto Guess Encoding，可以自动识别文件的编码方式。

主题（Theme）颜色我换了一圈，又换回Dark+(default dark)了。

在Extensions中，可以看到VSCode内置（Show Build-in Extensions）支持许多语言，但是只到基础语言（Language Basics）级别，这些基础语言级别不支持格式化文档（Format Document）。
完整语言特性（Language Features）支持的只有5种语言：
CSS Language Features
HTML Language Features
JSON Language Features
PHP Language Features，需要按照PHP的可执行文件
TypeScript And JavaScript Language Features
因为VSCode是使用JavaScript开发完成的，因此我们使用JavaScript做为默认语法高亮和格式化文档的语法工具。
当然，也可以自己安装各种语言的扩展组件完成语法高亮和格式化文档的功能。

常用快捷键（Keyboard Shortcuts）：
默认，不需要修改：
匹配（{}、[]或（））括号（Go to Bracket）：Ctrl + Shift + \
当前行上插入一行（Insert Line Above）：Ctrl + Shift + Enter
当前行下插入一行（Insert Line Below）：Ctrl + Enter
列（块）选中：
Alt + 鼠标左键点选
Alt + Shift + 鼠标左键拖动，或鼠标中键（滚轮键按压）进行列选择

需要修改的：
转大写（Transform to Uppercase）：Ctrl + Shift + U
转小写（Transform to Lowercase）：Ctrl + U

JavaScript格式化文档（也包括很多其他编辑器），都规定：
if、while、for这3个关键字的后面需要紧跟一个空格，然后才是左（圆）括号，和布尔表达式（条件判断语句）。
其实，在早期的编程格式建议中，甚至教科书中，并没有这个规定，我开始并不太适应这个建议，觉得没有必要。
大约2010年之后应该是随着脚本语言的兴起，这个编程规范变得开始普遍起来。
这个建议个人感觉上，应该主要是为了应对脚本语言的写法，一般脚本语言都是这么写，比如Lua：
if 布尔表达式 then end
while 布尔表达式 do end
for i = 1, 10 do end
脚本语言在这3大语句中，很少使用左（圆）括号，多使用空格进行间隔，很可能是这种脚本语言的潮流影响了类似C++这类语言的编程规范，客观的讲，打空格比打左（圆）括号更方便——空格是最大的按键。

能接受潮流，跟随潮流风格的变化，我觉得应该是一种进步。

Json的格式化程序（formatter）的配置是不开放的，不能像JavaScript那样改变settings.json中的配置（有发现可以的同学，请留言告诉我）。所以，要么接受官方默认格式，要么安装插件。

其实，代码风格这种东西，我认为：没有真正的哪种风格更好，关键是要统一，这个才是最重要的。

其实，仅仅是风格统一这一点，其实是很难做到的。

——“我早该想到的”

参考：
1.
《vscode 中增加文件后缀类型的支持: 设置cpp支持.cu等后缀》
https://blog.csdn.net/billbliss/article/details/82774315
2.
《2018 vscode 前端最佳配置》
https://www.jianshu.com/p/3f575ecb6161
3.
《vscode支持哪些编程语言》
https://www.php.cn/tool/vscode/434847.html
4.
《VsCode 格式化代码大括号的调整》
https://blog.csdn.net/shenliang34/article/details/80678273

5.
《VSCode打开多个项目文件夹的解决方法》
https://blog.csdn.net/magic_xiang/article/details/84024493
6.
《vscode 如何快速找到最近打开文件列表？》
https://www.v2ex.com/amp/t/561804
————————————————
版权声明：本文为CSDN博主「PeterNote」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/cpdoor2163_com/article/details/105920757