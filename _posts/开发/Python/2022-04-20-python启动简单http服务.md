---
tags:
    - Python
---

简单 HTTP 服务

SimpleHTTPServer是Python 2自带的一个模块，是Python的Web服务器。在Python 3已经合并到http.server模块中。如果不指定端口号默认的是8000端口。在局域网中使用web去访问http:/IP:8000即可
python2语法：

```
python -m SimpleHTTPServer
python -m SimpleHTTPServer 8000
```



python3语法：

```
python -m http.server

python -m http.server  1888
```





也可以在语句后门添加特定端口例如1234，在局域网中去使用web进行访问http://IP:1234即可





SimpleHTTPServer有一个特性，如果待共享的目录下有index.html，那么index.html文件会被视为默认主页；如果不存在index.html文件，那么就会显示整个目录列表。
————————————————
版权声明：本文为CSDN博主「*——*」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/youshijifen/article/details/88953199

