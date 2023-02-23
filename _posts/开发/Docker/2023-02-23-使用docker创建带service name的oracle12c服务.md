---
tags:
    - Docker
---

使用docker创建带service name的oracle12c服务

```
docker run -d --name oracle-12c-ee-service_name  --privileged   -p 8080:8080 -p 1521:1521 absolutapps/oracle-12c-ee
 
 ```
 
https://hub.docker.com/r/absolutapps/oracle-12c-ee

![e47b30b0e1387695e0ec2e273f224f53.png](/img-post/开发/Docker/使用docker创建带service name的oracle12c服务.assets/517f1409797f4e28850a314e46089e83.png)

IP：192.168.6.52
端口：1521
服务名：orcl
角色：默认
用户名：system
密码：oracle
可以在SYSTEM空间中操作。



如果需要修改SERVICE NAME等配置，可以进入docker中修改/u01/app/oracle/product/12.1.0.2/dbhome_1/network/admin/tnsnames.ora，配置参考：

$ more tnsnames.ora
ORCL=localhost:1521/ORCL
ORCLPDB1=
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 0.0.0.0)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = ORCLPDB1)
    )
  )
[oracle@1ca6d224e7e0 admin]$ pwd
/opt/oracle/product/12.2.0.1/dbhome_1/network/admin
