---
tags:
    - 操作系统
    - Mac
---

Mac OS下使用Privoxy做中转代理

由于工作原因，不得不需要访问google、github等墙外网站，Shadowsocks基本上解决了大部分问题（近期不知道为什么，偶尔不能正常使用）。但像git，ios设备不能享受到Shadowsocks带来的福利。偶然发现Privoxy，可以解决git和ios翻墙问题。

先说下为什么git和ios设备不能直接使用Shadowsocks，Shadowsocks的代理使用的是SOCKS5,git代理只支持HTTP代理，而ios下的Shadowsocks只能实现在软件内代理，而设置代理同样也只支持HTTP代理。使用Privoxy，可以把SOCKS5代理，转成HTTP代理。这样就可以提供给git和ios设备使用了。

安装Privoxy

使用brew就可以一键安装：brew install privoxy

如果提示-bash: brew: command not found 就说明你没有安装brew，不用担心，先安装一下brew。安装方法也很简单，终端里输入下面代码：

ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"


配置config文件，MAC下config文件的位置在： /usr/local/etc/privoxy/config

打开配置文件，在最下面添加：

listen-address 0.0.0.0:8118
forward-socks5 / 127.0.0.1:1080 .


1080是Shadowsocks代理的端口，这个改成你的端口。8118是开启http代理的端口。使用0.0.0.0即可在局域网内使用此代理，如只想本机使用，使用127.0.0.1。

开启Privoxy, 打开终端，输入 /usr/local/sbin/privoxy /usr/local/etc/privoxy/config 回车，没有报错就表示成功了。

实现Privoxy开机启动

打开终端，切换目录到 /Library/LaunchAgents，输入cd /Library/LaunchAgents加回车。 输入sudo touch local.privoxy.plist创建文件。输入sudo vim local.privoxy.plist进入编辑。

复制以下内容后，按esc键，再输入:wq回车保存退出。

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>local.arcueid.privoxy</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/sbin/privoxy</string>
        <string>--no-daemon</string>
        <string>/usr/local/etc/privoxy/config</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
    <key>StandardErrorPath</key>
    <string>/usr/local/Cellar/privoxy/privoxy.log</string>
    <key>StandardOutPath</key>
    <string>/usr/local/Cellar/privoxy/privoxy.log</string>
</dict>
</plist>


编辑完成后，执行如下命令，就可以把privoxy设置成开机自动启动了：



sudo launchctl load /Library/LaunchAgents/local.privoxy.plist


验证是否成功，在终端中输入netstat -an | grep 8118，如出现如下结果表示已经成功

tcp4       0      0  *.8118                 *.*                    LISTEN


给git配置代理

使用代理，终端中输入：git config --global http.proxy 127.0.0.1:8118

不使用代理，终端中输入：git config --global --unset-all http.proxy

IOS等第三方设备使用代理

![](/img-post/开发/操作系统/Mac/Mac OS下使用Privoxy做中转代理.assets/2566885e83c54a3db7a139503c972295.png)



