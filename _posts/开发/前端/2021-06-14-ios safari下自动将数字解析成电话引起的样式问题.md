---
tags:
    - 前端
---

ios safari下自动将数字解析成电话引起的样式问题

ios safari下会将数字转成电话号码，比如400-400-400会转成<a href="tel:400-400-400">400-400-400</a>



这样的话可能会因为a标签本身的样式导致数字显示的样式不正确



可以在head中增加<meta name="format-detection" content="telephone=no">禁止解析电话号码



如果不加这一行要考虑这里变成a标签后样式是否会有问题





http://stackoverflow.com/questions/26713165/removing-date-time-styling-from-html-email-iphone

