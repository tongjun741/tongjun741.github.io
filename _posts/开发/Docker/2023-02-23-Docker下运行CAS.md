---
tags:
    - Docker
---

Docker下运行CAS



一、完整的部署步骤

拉取镜像：

```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"textAlign":"left","verticalAlign":"left","backColor":"initial","value":"docker pull apereo/cas:v5.3.9","inlineStyles":{"font-family":[{"from":0,"to":29,"value":"Courier New"}],"color":[{"from":26,"to":29,"value":"#009900"}]}}],"heights":[40],"widths":[1389]}
```



启动cas容器时会自动去https://repository.apache.org和https://repo.maven.apache.org/下载jar包，直接下载非常慢，建议提前设置好docker的全局代理：

![](/img-post/开发/Docker/Docker下运行CAS.assets/c33360ef4e5847d888f36a6dccb3bbd7.png)

第一次启动容器：

```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"textAlign":"left","verticalAlign":"left","backColor":"initial","value":"docker run  --name cas -p 8443:8443 -p 8878:8080  apereo/cas:v5.3.9","inlineStyles":{"font-family":[{"from":0,"to":67,"value":"Courier New"}],"color":[{"from":26,"to":30,"value":"#009900"},{"from":31,"to":35,"value":"#009900"},{"from":39,"to":43,"value":"#009900"},{"from":44,"to":48,"value":"#009900"},{"from":64,"to":67,"value":"#009900"}]}}],"heights":[40],"widths":[1389]}
```

这时会自动下载war包并进行初始化，时间会比较久。启动结束后可以关闭Docker的全局代理。

第一次启动会因为没有证书而启动失败并自动退出，需要在宿主机上执行以下命令生成证书：

```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"textAlign":"left","verticalAlign":"left","backColor":"initial","value":"cd /tmp\nkeytool -genkeypair -alias cas -keyalg RSA -keypass changeit \\\n        -storepass changeit -keystore ./thekeystore \\\n        -dname \"CN=cas.example.org,OU=Example,OU=Org,C=AU\" \\\n        -ext SAN=\"dns:example.org,dns:localhost,ip:127.0.0.1\"","inlineStyles":{"font-family":[{"from":0,"to":7,"value":"Courier New"},{"from":8,"to":70,"value":"Courier New"},{"from":71,"to":124,"value":"Courier New"},{"from":125,"to":185,"value":"Courier New"},{"from":186,"to":247,"value":"Courier New"}],"color":[{"from":140,"to":183,"value":"#003366"},{"from":203,"to":247,"value":"#003366"}]}}],"heights":[40],"widths":[1389]}
```

重启容器并在容器自动退出前拷贝证书到容器：

```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"textAlign":"left","verticalAlign":"left","backColor":"initial","value":"docker restart cas\ndocker cp ./thekeystore cas:/etc/cas/thekeystore","inlineStyles":{"font-family":[{"from":0,"to":18,"value":"Courier New"},{"from":19,"to":67,"value":"Courier New"}]}}],"heights":[40],"widths":[1389]}
```

再次重启容器：

```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"textAlign":"left","verticalAlign":"left","backColor":"initial","value":"docker restart cas","inlineStyles":{"font-family":[{"from":0,"to":18,"value":"Courier New"}]}}],"heights":[40],"widths":[1389]}
```



等启动完成后需要进入容器修改配置：

```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"textAlign":"left","verticalAlign":"left","backColor":"initial","value":"docker exec -it cas /bin/bash\nmkdir /etc/cas/config\nvi /etc/cas/config/cas.properties\n \n \n#在新文件中增加以下内容在启动时创建允许连接的server信息：\ncas.serviceRegistry.initFromJson=true\n \n \n#增加以内容设置演示用户的登录信息，用户名密码以两个冒号分隔，多个用户以逗号分隔：\ncas.authn.accept.users = casuser::123456,admin::111","inlineStyles":{"bold":[{"from":156,"to":160,"value":true}],"font-family":[{"from":0,"to":29,"value":"Courier New"},{"from":30,"to":51,"value":"Courier New"},{"from":52,"to":85,"value":"Courier New"},{"from":86,"to":87,"value":"Courier New"},{"from":88,"to":89,"value":"Courier New"},{"from":90,"to":122,"value":"Courier New"},{"from":123,"to":160,"value":"Courier New"},{"from":161,"to":162,"value":"Courier New"},{"from":163,"to":164,"value":"Courier New"},{"from":165,"to":206,"value":"Courier New"},{"from":207,"to":258,"value":"Courier New"}],"color":[{"from":156,"to":160,"value":"#336699"},{"from":241,"to":247,"value":"#009900"},{"from":255,"to":258,"value":"#009900"}]}}],"heights":[40],"widths":[1389]}
```

保存后退出容器并重启容器：

```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"textAlign":"left","verticalAlign":"left","backColor":"initial","value":"docker restart cas","inlineStyles":{"font-family":[{"from":0,"to":18,"value":"Courier New"}]}}],"heights":[40],"widths":[1389]}
```





测试时需要门户服务器是使用https访问的，并且测试的用户在cas中配置了对应的用户名。

每次启动容器会重新打包，所以修改target目录下的内容是没办法生效的，只能是在/etc/cas/放置需要覆盖的配置文件。

如果需要禁止每次启动容器时重新打包可以修改容器中/cas-overlay/build.sh中的run函数，删除package命令。



未测试http访问模式的门户，可以参考https://blog.csdn.net/weixin_39206782/article/details/80871059



二、直接使用打包好的镜像

目前已将以上操作打包成了一个新的镜像并放到阿里云上了，使用方法：

```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"textAlign":"left","verticalAlign":"left","backColor":"initial","value":"docker pull registry.cn-shenzhen.aliyuncs.com/aaa/cas\ndocker run  --name cas -p 8443:8443 registry.cn-shenzhen.aliyuncs.com/aaa/cas\n \n \n#包含两个用户：casuser::123456,admin::111","inlineStyles":{"font-family":[{"from":0,"to":61,"value":"Courier New"},{"from":62,"to":147,"value":"Courier New"},{"from":148,"to":149,"value":"Courier New"},{"from":150,"to":151,"value":"Courier New"},{"from":152,"to":186,"value":"Courier New"}],"color":[{"from":88,"to":92,"value":"#009900"},{"from":93,"to":97,"value":"#009900"},{"from":169,"to":175,"value":"#009900"},{"from":183,"to":186,"value":"#009900"}]}}],"heights":[40],"widths":[1389]}
```

支持http协议访问门户：

```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"textAlign":"left","verticalAlign":"left","backColor":"initial","value":"#启动cas容器后执行以下命令进行容器的shell环境：\ndocker exec -it cas /bin/bash\nmkdir /etc/cas/services\ncp /cas-overlay/target/cas/WEB-INF/classes/services/HTTPSandIMAPS-10000001.json  /etc/cas/services\n \nvi /etc/cas/config/cas.properties\n#在文件中增加以下内容在启动时通过指定路径加载service信息：\ncas.serviceRegistry.json.location:    file:/etc/cas/services\n \n \n#退出并重启容器\nexit\ndocker restart cas","inlineStyles":{"font-family":[{"from":0,"to":28,"value":"Courier New"},{"from":29,"to":58,"value":"Courier New"},{"from":59,"to":82,"value":"Courier New"},{"from":83,"to":181,"value":"Courier New"},{"from":182,"to":183,"value":"Courier New"},{"from":184,"to":217,"value":"Courier New"},{"from":218,"to":251,"value":"Courier New"},{"from":252,"to":312,"value":"Courier New"},{"from":313,"to":314,"value":"Courier New"},{"from":315,"to":316,"value":"Courier New"},{"from":317,"to":325,"value":"Courier New"},{"from":326,"to":330,"value":"Courier New"},{"from":331,"to":349,"value":"Courier New"}],"color":[{"from":149,"to":157,"value":"#009900"}]}}],"heights":[40],"widths":[1389]}
```



三、使用CAS官方提供的Demo环境

```
原来格式为表格（table），转换较复杂，未转换，需要手动复制一下
{"cells":[{"textAlign":"left","verticalAlign":"left","backColor":"initial","value":"https://casserver.herokuapp.com/cas/login\n \n \n用户名：casuser\n密码：Mellon","inlineStyles":{"font-family":[{"from":0,"to":41,"value":"Courier New"},{"from":42,"to":43,"value":"Courier New"},{"from":44,"to":45,"value":"Courier New"},{"from":46,"to":57,"value":"Courier New"},{"from":58,"to":67,"value":"Courier New"}],"color":[{"from":6,"to":41,"value":"#008200"}]}}],"heights":[40],"widths":[1389]}
```



四、管理控制台中的配置方法

![](/img-post/开发/Docker/Docker下运行CAS.assets/2b4b99bcc6f44a39a17981f693b22146.png)

在管理控制台中配置好CAS信息后还需要在门户创建相同账号的用户，然后就可以开始测试了。



参考：



https://www.cnblogs.com/zhzhlong/p/11551361.html

https://github.com/apereo/cas-webapp-docker

https://dacurry-tns.github.io/deploying-apereo-cas/building_server_service-registry_configure-the-service-registry.html#define-the-service-registry-in-casproperties

https://groups.google.com/a/apereo.org/forum/#!topic/cas-user/L0XUII6FazI

https://apereo.github.io/2018/02/20/cas-service-rbac-attributeresolution/





