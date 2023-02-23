---
tags:
    - 其它
    - Nginx
---

购买完成，下载解压后，目录中有两个文件，一个是证书，另一个gd_bundle-g2-g1.crt

马上配置nginx

```
ssl on;
ssl_certificate /ssl_location/yourdomain.com.crt;
ssl_certificate_key /ssl_location/yourdomain.com.key;
```

配好后，使用firefox 出现上图错误。

推测是证书链断了。

google之，确实是证书链断了，需要把gd_bundle-g2-g1.crt和自己的证书保存为一个文件，供游览器向上爬证书链。方法如下：

cat yourdomain.com.crt gd_bundle-g2.crt >> yourdomain.com_combined.crt

重新配置nginx

```
ssl on;
ssl_certificate /ssl_location/yourdomain.com_combined.crt;
ssl_certificate_key /ssl_location/yourdomain.com.key;
```

之后再测试，测试通过。

解决过程中也找到了apache的配置方法，如下，apache支持证书链bundle文件

```
SSLEngine On
SSLCertificateFile /ssl_location/yourdomain.com.crt
SSLCertificateKeyFile /ssl_location/yourdomain.com.key
SSLCertificateChainFile /ssl_location/gd_bundle.crt
```

https://www.cnblogs.com/wloveh/p/4020232.html