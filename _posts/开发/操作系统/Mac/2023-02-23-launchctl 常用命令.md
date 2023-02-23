---
tags:
    - 操作系统
    - Mac
---

launchctl 常用命令



```javascript
1.显示当前的启动脚本
launchctl list

2.开机时自动启动Apache服务器
sudo launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist

3.设置开机启动并立即启动改服务
launchctl load -w   **.pist 

4. 设置开机启动但不立即启动服务 
launchctl load **.pist 

5. 停止正在运行的启动脚本
sudo launchctl unload [path/to/script]
6. 再加上-w选项即可去除开机启动
sudo launchctl unload -w [path/to/script]

```

https://www.jianshu.com/p/baa23cc820d2



```javascript
#!/bin/bash

lauchDirs=(
/Library/LaunchDaemons #系统启动时运行，用户不登录也会运行。
/Library/LaunchAgents #用户登录后运行。
~/Library/LaunchAgents #用户自定义的用户启动项
/System/Library/LaunchDaemons #系统自带的启动项
/System/Library/LaunchAgents #系统自带的启动项
)           ##要检查的地址
for ((i=0;i<${#lauchDirs[@]};i++))
do
    dir=${lauchDirs[$i]}
    echo $dir
    ls -l $dir | grep shadow
done

```



