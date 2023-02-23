---
tags:
    - 操作系统
    - Mac
    - 软件安装
---

mac php mongo

php-mongo分两个版本：

mongo和mongodb，mongodb是新版，与RockMongo1.1.7不适配

php-mongo的安装地址：

https://github.com/mongodb/mongo-php-driver-legacy



上次升级OSX之后，之前安装的 mongo 和 redis 的扩展都没有了，好像是php.ini 中的配置变了。并且那两个扩展都找不见了，所以只能重新安装一下。

主要内容就是下载对应的扩展，编译，然后安装。

编译的方法也比较简单，

phpize

./configure

make

sudo make install

最后会告诉你这个扩展安装在了什么地方。那个路径应该不回有错。

然后就是修改php.ini 中的配置，添加

extension=mongo.so

extension=redis.so

重启apache，然后，用phpinfo() 测试，就可以看到mongo 和 redis 这两个扩展了。

mongo 和 redis 扩展都可以从这里找到。下个比较新的 stable的就可以了。另外也要注意自己的php 的版本，我的php是5.5.14。

mongo 和 redis 的php扩展，可以到这个网站里找 http://pecl.php.net/在这个网站里自己搜索就可以了。

http://blog.csdn.net/typeof_/article/details/41911691

