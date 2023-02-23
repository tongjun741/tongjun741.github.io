---
tags:
    - 其它
    - Git
    - gitlab
---

证书自动续期暂时还没学会：
gitlab的证书应该是由gitlab.rb决定的（nginx中也有，它们的关系我还没搞懂，回头看官方文档）


一、查看/etc/gitlab/gitlab.rb
root@gitlab1:/etc/gitlab# grep ssl /etc/gitlab/gitlab.rb | grep -v "#"
nginx['ssl_certificate'] = "/etc/letsencrypt/live/gitlab.x.xxxxx.com/fullchain.pem"
nginx['ssl_certificate_key'] = "/etc/letsencrypt/live/gitlab.x.xxxxx.com/privkey.pem"
二、手动生成证书，需要公网ip机器，且80端口没有nginx或apache在用（见上一篇文章）
root@david-test:~# acme.sh --issue -d gitlab.x.xxxxx.com  --dns dns-01
[Thu Dec  6 06:26:20 UTC 2018] Registering account
[Thu Dec  6 06:26:21 UTC 2018] Registered
[Thu Dec  6 06:26:21 UTC 2018] ACCOUNT_THUMBPRINT='hcohD58qkaMXyYWyXW3ZK_Xjd3A-FDFJW_It-bupspc'
[Thu Dec  6 06:26:21 UTC 2018] Creating domain key
[Thu Dec  6 06:26:21 UTC 2018] The domain key is here: /root/.acme.sh/gitlab.x.xxxxx.com/gitlab.x.xxxxx.com.key
[Thu Dec  6 06:26:21 UTC 2018] Single domain='gitlab.x.xxxxx.com'
[Thu Dec  6 06:26:21 UTC 2018] Getting domain auth token for each domain
[Thu Dec  6 06:26:21 UTC 2018] Getting webroot for domain='gitlab.x.xxxxx.com'
[Thu Dec  6 06:26:21 UTC 2018] Getting new-authz for domain='gitlab.x.xxxxx.com'
[Thu Dec  6 06:26:22 UTC 2018] The new-authz request is ok.
[Thu Dec  6 06:26:22 UTC 2018] Can not find dns api hook for: dns-01
[Thu Dec  6 06:26:22 UTC 2018] You need to add the txt record manually.
[Thu Dec  6 06:26:22 UTC 2018] Add the following TXT record:
[Thu Dec  6 06:26:22 UTC 2018] Domain: '_acme-challenge.gitlab.x.xxxxx.com'
[Thu Dec  6 06:26:22 UTC 2018] TXT value: '1nGCiUqKBZ6ZVu1X2Ne9ueu5EZ7OKBUCGnRZKcjUcnw'
[Thu Dec  6 06:26:22 UTC 2018] Please be aware that you prepend _acme-challenge. before your domain
[Thu Dec  6 06:26:22 UTC 2018] so the resulting subdomain will be: _acme-challenge.gitlab.x.xxxxx.com
[Thu Dec  6 06:26:22 UTC 2018] Please add the TXT records to the domains, and re-run with --renew.
[Thu Dec  6 06:26:22 UTC 2018] Please add '--debug' or '--log' to check more details.
[Thu Dec  6 06:26:22 UTC 2018] See: https://github.com/Neilpang/acme.sh/wiki/How-to-debug-acme.sh


添加txt解析记录：


再次执行命令：
root@david-test:~# acme.sh --issue -d gitlab.x.xxxxx.com  --dns dns-01 --renew
[Thu Dec  6 06:29:05 UTC 2018] Renew: 'gitlab.x.xxxxx.com'
[Thu Dec  6 06:29:06 UTC 2018] Single domain='gitlab.x.xxxxx.com'
[Thu Dec  6 06:29:06 UTC 2018] Getting domain auth token for each domain
[Thu Dec  6 06:29:06 UTC 2018] Verifying:gitlab.x.xxxxx.com
[Thu Dec  6 06:29:10 UTC 2018] Success
[Thu Dec  6 06:29:10 UTC 2018] Verify finished, start to sign.
[Thu Dec  6 06:29:11 UTC 2018] Cert success.
-----BEGIN CERTIFICATE-----
......省略一大段废话......
-----END CERTIFICATE-----
[Thu Dec  6 06:29:11 UTC 2018] Your cert is in  /root/.acme.sh/gitlab.x.xxxxx.com/gitlab.x.xxxxx.com.cer 
[Thu Dec  6 06:29:11 UTC 2018] Your cert key is in  /root/.acme.sh/gitlab.x.xxxxx.com/gitlab.x.xxxxx.com.key 
[Thu Dec  6 06:29:11 UTC 2018] The intermediate CA cert is in  /root/.acme.sh/gitlab.x.xxxxx.com/ca.cer 
[Thu Dec  6 06:29:11 UTC 2018] And the full chain certs is there:  /root/.acme.sh/gitlab.x.xxxxx.com/fullchain.cer

三、将fullchain.cer和.key文件拷到服务器上对应位置，名字改成和之前一样
四、关键一步：重启nginx
由于gitlab的nginx是集成在gitlab里面的，所以得使用gitlab的命令去重启nginx
root@gitlab1:~# gitlab-ctl restart nginx

————————————————
版权声明：本文为CSDN博主「USCWIFI」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_33317586/article/details/84854582