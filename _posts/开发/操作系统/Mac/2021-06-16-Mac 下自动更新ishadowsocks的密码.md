---
tags:
    - 操作系统
    - Mac
---

Mac 下自动更新ishadowsocks的密码

1.安装shadowsocks的客户端:

$ brew install shadowsocks-libev



将/usr/local/Cellar/shadowsocks-libev/2.4.5/bin加入path



2.clone ssflow:

$ git clone https://github.com/youfou/ssflow.git



3.配置ssflow:

$ cd ssflow

$ cp ssflow.config.example ssflow.config

$ vim ssflow.config



配置以下项

[shadowsocks_libev_osx]

# ss-local 的路径

ss_local_path=ss-local

# PAC 文件的路径，用于 "auto" 模式

pac_path=~/.ShadowsocksX/gfwlist.js



注意ss_local_path指的是shadowsocks-libev的可执行程序，需要能直接访问。

pac_path必须存在。

如果觉得测试时间太长可以将ping.deadline改小，如：

deadline=3



4.修改ss源和客户端类型:

$ vim main.py



# coding: utf8





import common

from sources import iShadowsocks

from targets import Shadowsocks_libev_OSX





def deploy(source, target):

    nodes = source().get_nodes()

    nodes.test()

    nodes.deploy_to(target())

    common.log.info('Done!’)



5.启动:

$ python main.py



6.定时更新密码:

$ vim ~/autoSS.sh



#!/bin/bash

pkill python /Users/user/workspace/ssflow/main.py

nohup python /Users/user/workspace/ssflow/main.py >/dev/null 2>&1 &



$ crontab -e



#自动更新ishadowsocks的密码

0-3 */6 * * * ~/autoSS.sh







if __name__ == '__main__':

    deploy(iShadowsocks, Shadowsocks_libev_OSX)

