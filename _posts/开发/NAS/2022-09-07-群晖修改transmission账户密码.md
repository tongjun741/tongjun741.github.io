---
tags:
    - NAS
---

#### 1.停止transmission

[![img](/img-post/开发/NAS/群晖修改transmission账户密码.assets/20201123110851.jpg)](https://hibb.dusays.com/wp-content/uploads/2020/11/20201123110851.jpg)

#### 2.开启SSH putty登录到群晖

[![img](/img-post/开发/NAS/群晖修改transmission账户密码.assets/20201123111130.jpg)](https://hibb.dusays.com/wp-content/uploads/2020/11/20201123111130.jpg)

#### 3.然后sudo -i获取root账户权限(过程中密码不会显示)

[![img](/img-post/开发/NAS/群晖修改transmission账户密码.assets/20201123111219.jpg)](https://hibb.dusays.com/wp-content/uploads/2020/11/20201123111219.jpg)

#### 4. 查找settings.json

[code]find / -name settings.json[/code]

[![img](/img-post/开发/NAS/群晖修改transmission账户密码.assets/20201123111303.jpg)](https://hibb.dusays.com/wp-content/uploads/2020/11/20201123111303.jpg)

#### 5. 修改密码

[code]vim /volume1/@appstore/transmission/var/settings.json[/code]

找到”[code]rpc-password[/code]”

后面引号内就是经过加密的密码，不要管他怎么加密的，直接把引号内的内容修改为你的新密码就可以了，比如”rpc-password”:”xxorg.com”

然后按“ESC”键，输入：wq

[![img](/img-post/开发/NAS/群晖修改transmission账户密码.assets/20201123111547.jpg)](https://hibb.dusays.com/wp-content/uploads/2020/11/20201123111547.jpg)

如果按esc退不出编辑模式的话，试试 ctrl+c可以，还有什么按两个大Z的

6.最后到群晖套件中心启动transmission，就可以了，账户密码都已经修改过来了



https://www.hibbba.com/5253.html