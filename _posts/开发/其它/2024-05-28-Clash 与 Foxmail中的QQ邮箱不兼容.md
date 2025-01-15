---
tags:
    - 其它
---

Clash 开启 tun模式 和系统代理后Foxmail中的QQ邮件无法收取邮件。

以下设置似乎可用：

![image-20240528165919470](/img-post/开发/其它/Clash 与 Foxmail中的QQ邮箱不兼容.assets/image-20240528165919470.png)

![image-20240528165937391](/img-post/开发/其它/Clash 与 Foxmail中的QQ邮箱不兼容.assets/image-20240528165937391.png)

![image-20240528170051647](/img-post/开发/其它/Clash 与 Foxmail中的QQ邮箱不兼容.assets/image-20240528170051647.png)



另外需要在Parsers Script中设置qq.com直连：

```
        - DOMAIN-SUFFIX,qq.com,DIRECT
```

