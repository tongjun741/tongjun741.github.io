---
tags:
    - 其它
    - 油猴脚本
---

```
// ==UserScript==
// @name         自动测试
// @namespace    http://tampermonkey.net/
// @version      2024-05-24
// @description  try to take over the world!
// @author       You
// @match        https://aa.bb.com/*
// @icon         data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==
// @grant        none
// @require      https://cdn.staticfile.org/jquery/3.4.1/jquery.min.js
// @require      file://D:\aa\自动测试.js
// ==/UserScript==

```



D:\aa\自动测试.js：

```
alert(111)
```



默认情况下会有权限问题：

```
@require: couldn't load @require from URL 'file://D:\aa\自动测试.js': Access to this or all local files is forbidden!
```



需要修改“篡改猴”插件的权限，勾选“允许访问文件网址”。

![image-20240524160631797](/img-post/开发/其它/油猴脚本/在油猴脚本中使用require加载本地JavaScript文件并执行.assets/image-20240524160631797.png)



https://stackoverflow.com/questions/68205265/why-does-require-always-give-me-the-error-couldnt-load-require-from-forbidde