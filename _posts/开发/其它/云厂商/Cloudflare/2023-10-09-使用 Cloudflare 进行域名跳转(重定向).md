---
tags:
    - 其它
    - 云厂商
    - Cloudflare
---

**1 在Cloudflare添加你的域名**

登陆 [https://www.cloudflare.com/](https://link.zhihu.com/?target=https%3A//dash.cloudflare.com/)，填好你的Cloudflare账号和密码，登陆进Cloudflare。


点击左侧菜单的“Websites”，在右侧的页面点击“Add a Site”按钮，添加自己的域名，如study.win

![img](/img-post/开发/其它/云厂商/Cloudflare/使用 Cloudflare 进行域名跳转(重定向).assets/v2-46602d0c649132e8cdf5bd5dc9016300_1440w.webp)

添加成功后，选择study.win这个域名，进入该域名的配置页

![img](/img-post/开发/其它/云厂商/Cloudflare/使用 Cloudflare 进行域名跳转(重定向).assets/v2-a7224b4c423e986ae4937a0b65d0ef4c_1440w.webp)

**2 配置DNS服务器**

a) 点击左侧菜单的“DNS”，看到系统为study.win这个域名生成的2个DNS服务器，如红框所示

![img](/img-post/开发/其它/云厂商/Cloudflare/使用 Cloudflare 进行域名跳转(重定向).assets/v2-94acfd481f7b9be02c0b349fc165795f_1440w.webp)



b) 然后进入study.win这个域名的注册商系统，把该域名的DNS改为cloudflare提供的DNS服务器，也就是使用上面红框所示的2个DNS服务器，具体操作省略。注意，修改后的新的DNS服务器可能需要等待一段时间才会生效。



**3 添加DNS Record**

回到Cloudflare系统，点击“Add record”按钮，按下图添加2条记录，一条记录是 @ 记录，另外一条是 www 记录，Type 都选 A，Name 就是填 @ 和 www，IPv4 address 随便写即可，因为后面是要做域名跳转的，这里填写的IPv4不是真正要跳转的URL。

注意，@记录在Name字段填 @成功添加记录后，会自动变为你的域名名字，如study.win

![img](/img-post/开发/其它/云厂商/Cloudflare/使用 Cloudflare 进行域名跳转(重定向).assets/v2-dc80131af2fd8897096aff8ad9097348_1440w.webp)



**4 添加页面规则**

点击左侧菜单的“Page Rules”，按下图添加2条规则，

![img](/img-post/开发/其它/云厂商/Cloudflare/使用 Cloudflare 进行域名跳转(重定向).assets/v2-3f77b9b10f97f2f7381c2bb41584086f_1440w.webp)

URL (required) 字段填 study.win/* 和 www.study.win/*

Pick a Setting (required)字段选 “Forwarding URL”

Select status code (required)字段选 “301 Permanent Redirect”

Enter destination URL (required) 字段填写你要跳转的目标URL


然后点击“Save”按钮



最后创建后的2条规则展示如下：

![img](/img-post/开发/其它/云厂商/Cloudflare/使用 Cloudflare 进行域名跳转(重定向).assets/v2-3d00d7a245e9d8ed9f09631a7ec1d7ab_1440w.webp)

这样就完成域名跳转的配置了。具体的规则配置帮助可以参考

[https://support.cloudflare.com/hc/zh-cn/articles/218411427](https://link.zhihu.com/?target=https%3A//support.cloudflare.com/hc/zh-cn/articles/218411427)



在浏览器里输入study.win或www.study.win，就能跳转到你的目标URL





https://zhuanlan.zhihu.com/p/495264377