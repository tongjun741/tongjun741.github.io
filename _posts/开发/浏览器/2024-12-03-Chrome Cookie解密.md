---
tags:
    - 浏览器
---



Chrome所有域名下的所有Cookie数据，实际的本地存储文件路径是：
`C:\Users\youbl\AppData\Local\Google\Chrome\User Data\Default\Network\Cookies`

解密密钥在Local State文件：
C:\Users\youbl\AppData\Local\Google\Chrome\User Data\Local State
这是一个明文的Json格式文件，密钥是是base64后存储在json的os_crypt.encrypted_key属性里，你可以自行用记事本或json查看工具，打开这个文件查看内容。



可以通过以下脚本解密：

https://github.com/tiennm99/export-chrome-cookies/blob/main/main.py

## 注意一定要在同一台电脑上运行解密程序，拷贝到其它电脑上执行是不行的。





https://blog.csdn.net/youbl/article/details/139707917