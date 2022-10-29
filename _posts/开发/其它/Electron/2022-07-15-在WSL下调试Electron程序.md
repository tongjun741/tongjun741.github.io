---
tags:
    - 其它
    - Electron
---

下载VcXsrv ： https://sourceforge.net/projects/vcxsrv/files/latest/download

### VcXsrv Config

Open `VcXsrv` and it’ll guide you through three config screens. Here’s what to pick on each one:

1. Choose Multiple Windows
2. Choose ‘Start no client’
3. Choose Clipboard and OpenGL integration, plus provide `-ac` as additional arguments

![img](/img-post/开发/其它/Electron/在WSL下调试Electron程序.assets/vcx-1-3ee1bba39c26369340e6af06c883fb54368cda311b43c39b2e29e3736c1399e7.png)

![img](/img-post/开发/其它/Electron/在WSL下调试Electron程序.assets/vcx-2-315ebc9e09f9e5ea85fcc20884ecb613b6c0c5140ddc6f093ad9e92c6a66c54a.png)

![img](/img-post/开发/其它/Electron/在WSL下调试Electron程序.assets/vcx-3-d932bfd3461c30edffef28722d9e7f4ad27c29a9120530ee517841c2da382bab.png)

Save your config in your home directory for eash launching, then start the server. You’ll see the little `X` logo in your system tray. We’re ready to go.

### WSL Config

```
# Add the following lines to your ~/.bashrc file:
# 如果使用的是zsh，需要修改.zshrc，可通过  echo $SHELL  确认

export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0
export LIBGL_ALWAYS_INDIRECT=true
```

解决中文乱码

```
sudo apt-get -y install locales xfonts-intl-chinese fonts-wqy-microhei  -y
```



To test that you have everything working, you can try starting an app. Lets try the basic x11 apps to make sure everything is working.

```
sudo apt install x11-apps -y

xcalc
```

You should see the gorgeous x11 calc window.

![img](/img-post/开发/其它/Electron/在WSL下调试Electron程序.assets/xcalc-ab2c6e4475a874ab280cb9eb0b59f5aa9ab31fd9c5fa57d0e94c9a8c1cec6761.png)

Hey, that actually looks pretty good! Who needs the new Windows calculator anyway.

Oh wait, do you have a Hi-DPI screen and everything looks blurry? Ok you should follow [this Stackoverflow post](https://superuser.com/questions/1370361/blurry-fonts-on-using-windows-default-scaling-with-wsl-gui-applications-hidpi) to stop the blur, or alternatively use [X410](https://x410.dev/) instead of vcsXsrv which supports hiDPI screens out of the box (but is a paid app). I simply disabled all dpi-scaling for vcsXsrv windows as I only have a 1080 screen.

### Running Our Electron App

Now we can run our electron dev server again.

```
yarn run electron:start
```





https://www.beekeeperstudio.io/blog/building-electron-windows-ubuntu-wsl2

https://www.jianshu.com/p/1257932ef19f

https://www.cnblogs.com/zhengzh/p/6757752.html
