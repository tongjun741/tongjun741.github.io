---
tags:
    - 其它
    - Fiddler
---

在浏览器中访问fiddler的代理ip加端口下载证书，得到的是FiddlerRoot.cer，需要转成crt：

```
openssl x509 -inform der -in ~/Downloads/FiddlerRoot.cer -out ~/Downloads/FiddlerRoot.crt
```

然后使用下面的命令开启证书管理：

```
sudo cp ~/Downloads/FiddlerRoot.crt  /usr/share/ca-certificates/
sudo dpkg-reconfigure ca-certificates
```

选择FiddlerRoot.crt，点击OK。

参考：https://unix.stackexchange.com/questions/353731/how-can-i-install-fiddler-ca-certificate-on-ubuntu-to-decrypt-https



#### . Firefox 浏览器（独立证书管理）

Firefox 不使用系统证书库，需单独配置：



- 地址栏输入 `about:preferences#privacy`

- 下滑到 **证书** 部分，点击 **查看证书**

- 切换到 **证书机构** 选项卡，点击 **导入**

- 选择

   

  ```
  FiddlerRoot.cer
  ```

  ，勾选：

  - 信任此 CA 标识网站

- 点击 **确定**