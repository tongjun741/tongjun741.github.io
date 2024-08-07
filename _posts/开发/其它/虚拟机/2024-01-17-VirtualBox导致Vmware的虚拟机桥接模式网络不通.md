---
tags:
    - 其它
    - 虚拟机
---

[VMware](https://so.csdn.net/so/search?q=VMware&spm=1001.2101.3001.7020)内的虚拟机，设置为桥接后，无法连接外网。
物理主机IP地址：192.168.0.60，虚拟机IP地址：192.168.0.61，网关地址：192.168.0.1
虚拟机网络采用[桥接模式](https://so.csdn.net/so/search?q=桥接模式&spm=1001.2101.3001.7020)：


从物理机无法ping通虚拟机，虚拟机也无法ping通网关。

后来经过检查，发现是因为在物理机上同时还安装了VirtualBox导致的。
从“编辑”-“虚拟网络编辑器”菜单进入，查看“桥接模式”的“自动配置”，将VirtualBox从自动桥接的主机网络适配器中去除，问题解决。

 ![image-20240117154639387](/img-post/开发/其它/虚拟机/VirtualBox导致Vmware的虚拟机桥接模式网络不通.assets/image-20240117154639387.png)

转载至https://blog.csdn.net/itsgoodtobebad/article/details/78515354