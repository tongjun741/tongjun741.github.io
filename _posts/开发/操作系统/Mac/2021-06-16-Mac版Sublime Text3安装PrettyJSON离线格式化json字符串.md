---
tags:
    - 操作系统
    - Mac
---

Mac版Sublime Text3安装PrettyJSON离线格式化json字符串

官网下载 Sublime Text Build 3103.dmg安装

安装install package



在线安装：点击菜单中的 “View”→“Show Console”（或快捷键 Ctrl + ` ）把下面的代码粘贴进去后回车即可：



import urllib.request,os,hashlib; h = ‘6f4c264a24d933ce70df5dedcf1dcaee’ + ‘ebe013ee18cced0ef93d5f746d80ef60’; pf = ‘Package Control.sublime-package’; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( ‘http://packagecontrol.io/’ + pf.replace(’ ‘, ‘%20’)).read(); dh = hashlib.sha256(by).hexdigest(); print(‘Error validating download (got %s instead of %s), please try manual install’ % (dh, h)) if dh != h else open(os.path.join( ipp, pf), ‘wb’ ).write(by)



离线安装：下载Package Control.sublime-package，然后将其放到/Users/yanzi/Library/Application Support/Sublime Text 3/Installed Packages 路径下，之后重启！



安装Rretty Json 

使用 Command（或ctrl）+ Shift + P 调出面板，然后输入pci ，选中“Package Control: Install Package”并回车，然后通过输入插件的名字pretty json找到插件并回车安装即可。

使用command + ctrl + j快捷键来格式化当前页面的内容

--------------------- 

作者：坏小虎 

来源：CSDN 

原文：https://blog.csdn.net/hmily_lcq/article/details/79567210 

版权声明：本文为博主原创文章，转载请附上博文链接！

