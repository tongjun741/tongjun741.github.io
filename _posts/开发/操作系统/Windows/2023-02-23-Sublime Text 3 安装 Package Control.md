---
tags:
    - 操作系统
    - Windows
---

Sublime Text 3 安装 Package Control

自动安装：

1、通过快捷键 ctrl+` 或者 View > Show Console 菜单打开控制台

2、粘贴对应版本的代码后回车安装

适用于 Sublime Text 3：

import  urllib.request,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();urllib.request.install_opener(urllib.request.build_opener(urllib.request.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib.request.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())

适用于 Sublime Text 2：

import  urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp)ifnotos.path.exists(ipp)elseNone;urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read());print('Please restart Sublime Text to finish installation')

手动安装：

第一步，先下载https://github.com/wbond/sublime_package_control中的zip文件，解压后将文件夹名更改为package control。

第二步，下载插件分支https://github.com/wbond/sublime_package_control/tree/python3中的zip文件，解压后覆盖到package control中，完成此插件API函数的更新。

最后，将package control文件夹放入C:\Users\Mr.DenGo(你的电脑名)\AppData\Roaming\Sublime Text 3\Packages中，重启sublime text 3即可生效。

sublime text 3常用插件推荐：

ctrl+shift+p调用出窗口：输入install package

然后输入你想要的插件即可:

BracketHighlighter高亮显示匹配的括号、引号和标签

BracketHighlighter这个插件能在左侧高亮显示匹配的括号、引号和标签，能匹配的[] ,  () ,  {} ,  "" ,  '' , <tag></tag>等甚至是自定义的标签，当看到密密麻麻的代码分不清标签之间包容嵌套的关系时，这款插件就能很好地帮你理清楚代码结构，快速定位括号，引号和标签内的范围。

插件下载： https://github.com/facelessuser/BracketHighlighter/tree/BH2ST3

TrailingSpacer高亮显示多余的空格和Tab

有时候在代码结尾打多了几个空格或Tab，一般不会察觉，TrailingSpacer这款插件能高亮显示多余的空格和Tab，并可以一键删除它们，有代码洁癖的朋友应该会喜欢这个插件。

插件下载： https://github.com/SublimeText/TrailingSpaces

注意，在github上下载的插件缺少了一个设置快捷键的文件，可以新建一个名字和后缀为Default (Windows).sublime-keymap的文件，添加以下代码，即可设置“删除多余空格”和“是否开启TrailingSpacer ”的快捷键了。

[

{ "keys":["ctrl+alt+d"], "command":"delete_trailing_spaces"},



{ "keys":["ctrl+alt+o"], "command":"toggle_trailing_spaces"}]

Alignment 等号对齐

按Ctrl+Alt+A，可以是凌乱的代码以等号为准左右对其，适合有代码洁癖的朋友。

插件下载： https://github.com/kevinsperrine/sublime_alignment/tree/python3

Clipboard-history 粘贴板历史记录

有了这个插件，便可方便使用sublime text 3里的粘贴板历史记录内容，快捷键Ctrl+Shift+V可调出该历史记录面板，按方向键选择想要粘贴的历史记录。不过这是sublime text 2下的插件，Ctrl+Shift+D清除粘贴板历史记录好像不能生效，不过重启sublime也可清除粘贴板的历史记录。

插件下载： https://github.com/kemayo/sublime-text-2-clipboard-history

SideBarEnhancements侧边栏增强

SideBarEnhancements本是增强侧边栏的插件，这里将教大家如何用来做sublime text 3浏览器预览插件，并可自定义浏览器预览的快捷键。

安装此插件，点击工具栏的preferences > package setting > side bar > Key Building-User，键入以下代码，这里设置按Ctrl+Shift+C复制文件路径，按F1~F5分别在firefox，chrome，IE，safari，opera浏览器预览效果，当然你也可以自己定义喜欢的快捷键，最后注意代码中的浏览器路径要以自己电脑里的文件路径为准。

[

{ "keys":["ctrl+shift+c"], "command":"copy_path"},

//firefox

{ "keys":["f1"], "command":"side_bar_files_open_with",

"args":{

"paths":[],

"application":"C:\\software\\Browser\\Mozilla Firefox\\firefox.exe",

"extensions":".*"//匹配任何文件类型

}},

//chrome

{ "keys":["f2"], "command":"side_bar_files_open_with",

"args":{

"paths":[],

"application":"C:\\Users\\Mr.DenGo\\AppData\\Local\\Google\\Chrome\\Application\\chrome.exe",

"extensions":".*"}},

//ie

{ "keys":["f3"], "command":"side_bar_files_open_with",

"args":{

"paths":[],

"application":"C:\\Program Files\\Internet Explorer\\iexplore.exe",

"extensions":".*"}},

//safari

{ "keys":["f4"], "command":"side_bar_files_open_with",

"args":{

"paths":[],

"application":"C:\\software\\Browser\\Safari\\safari.exe",

"extensions":".*"}},

//opera

{ "keys":["f5"], "command":"side_bar_files_open_with",

"args":{

"paths":[],

"application":"C:\\software\\Browser\\opera\\opera.exe",

"extensions":".*"}}]





http://jingyan.baidu.com/article/925f8cb817fd49c0dce05653.html

