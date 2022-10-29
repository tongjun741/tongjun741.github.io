---
tags:
    - 操作系统
    - Linux
---

Ubuntu 18 开启vnc server



```javascript
sudo apt update && sudo apt install -y vino

gnome-control-center sharing # 开启桌面共享

ss -lnt # 检查5900有没有监听

gsettings set org.gnome.Vino require-encryption false # 关闭认证

gsettings list-recursively org.gnome.Vino | grep encrypt # 确认已经关闭  org.gnome.Vino require-encryption false

```



https://linuxconfig.org/ubuntu-remote-desktop-18-04-bionic-beaver-linux



https://www.jianshu.com/p/f58fe5cdeb5f

