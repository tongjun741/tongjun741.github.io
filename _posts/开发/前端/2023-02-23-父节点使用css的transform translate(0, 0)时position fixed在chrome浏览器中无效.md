---
tags:
    - 前端
---

父节点使用css的transform translate(0, 0)时position fixed在chrome浏览器中无效

今天在做移动端的页面，无意间发现了一个Chrome浏览器下的一个bug，在使用CSS3的transform: translate(0, 0)属性对节点A进行位置转化，此时A节点下面有一个字节点B，节点B使用了position:fixed进行了定位，按照常理节点B应该悬挂在浏览器窗口视图上，不会跟随滚动条而滚动的，但是这个效果在Chrome浏览器下面是无效的，经过测试在IE11、Firefox、safari中均没有问题，在Opera中出现的效果和Chrome中完全一样。

总结一下：在Chrome和Opera浏览器下，使用CSS3的transform: translate(0, 0)转化位置节点，其所有使用position:fixed定位的子孙节点的定位功能均无效。



http://www.cnblogs.com/yunfour/p/3928724.html

