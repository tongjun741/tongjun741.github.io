---
tags:
    - MongoDB
---

mongodb的连接问题，绑定IP惹的祸

使用localhost:27017连接不上，换成127.0.0.1:27017就能连接上了



看了/usr/local/etc/mongod.conf，发现bind_ip设置为了127.0.0.1，改成0.0.0.0就ok了



http://stackoverflow.com/questions/8904991/mongodb-cant-connect-to-localhost-but-can-connect-to-localhosts-ip-address



http://www.2cto.com/database/201309/245567.html

