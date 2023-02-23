---
tags:
    - 其它
    - Jenkins
---

进入 `$HOME/Library/LaunchAgents` 目录下，新建一个 `com.jenkins.agent.plist` （文件名可更改）文件，内容如下：



```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>KeepAlive</key>
  <true/>
  <key>Label</key>
  <string>com.jenkins.agent</string>
  <key>ProgramArguments</key>
  <array>
      <string>/usr/bin/java</string>
      <string>-jar</string>
      <string>/<your>/<path>/agent.jar</string>
      <string>-jnlpUrl</string>
      <string>http://jenkins.xxx.com/computer/<node-name>/slave-agent.jnlp</string>
      <string>-secret</string>
      <string><cipher></string>
      <string>-workDir</string>
      <string>节点的工作目录</string>
  </array>
  <key>RunAtLoad</key>
  <true/>
  <key>StandardOutPath</key>
  <string>/<your>/<log>/<path>/jenkins-stdout.log</string>
  <key>StandardErrorPath</key>
  <string>/<your>/<log>/<path>/jenkins-error.log</string>
  <key>WorkingDirectory</key>
  <string>节点的工作目录</string>
</dict>
</plist>
```

- 注意事项

  1. `WorkingDirectory` 此路径必须配置，否则 Jenkins 会抛出 IO 无权限错误

  2. ```
     ProgramArguments
     ```

      这里字段的配置项，实际上就对应着节点的启动命令，如下图

     ![img](https:////upload-images.jianshu.io/upload_images/4979007-c35af5906cc3cd1e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

     com.jenkins.agent.plist

     ![img](https:////upload-images.jianshu.io/upload_images/4979007-9c2a080d87da2060.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

     agent

1. 其它的字段无需更改
2. 启动服务：`launchctl load $HOME/Library/LaunchAgents/com.jenkins.agent.plist`
3. 终止服务：`launchctl unload $HOME/Library/LaunchAgents/com.jenkins.agent.plist`



作者：纳爱斯
链接：https://www.jianshu.com/p/b3ff4120f92c
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。