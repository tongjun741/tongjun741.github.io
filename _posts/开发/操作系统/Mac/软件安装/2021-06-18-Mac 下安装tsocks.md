---
tags:
    - 操作系统
    - Mac
    - 软件安装
---

Mac 下安装tsocks

tap repo: 

brew tap Anakros/homebrew-tsocks



install tsocks: 

brew install --HEAD tsocks



可能会link不成功，需要手工link:

sudo ln -s /usr/local/Cellar/tsocks/HEAD-562448f/bin/tsocks /usr/bin/tsocks



edit configuration file: 

vim /usr/local/etc/tsocks.conf

添加代码

server = 127.0.0.1 ＃代理地址
server_type = 5    ＃5即socks代理
server_port = 8080 ＃代理端口


然后即可在命令行对任意软件应用代理

$ tsocks wget https://google.com







https://github.com/Anakros/homebrew-tsocks



http://blog.aizhet.com/Apple/17446.html

