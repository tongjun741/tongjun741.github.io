---
tags:
    - 其它
    - Electron
---

现象：

```
 [34m•[39m electron-builder  [34mversion[39m=22.11.7 [34mos[39m=3.10.0-1160.25.1.el7.x86_64
  [34m•[39m artifacts will be published if draft release exists  [34mreason[39m=CI detected
  [34m•[39m loaded configuration  [34mfile[39m=/project/electron/electron-builder.json
[34m  • [0mrebuilding native dependencies  [34mdependencies[0m=better-sqlite3@7.4.5, ffi-napi@4.0.3, ref-napi@3.0.3 [34mplatform[0m=win32 [34march[0m=x64
[34m  • [0minstall prebuilt binary  [34mname[0m=better-sqlite3 [34mversion[0m=7.4.5 [34mplatform[0m=win32 [34march[0m=x64 [34mnapi[0m=
[31m  ⨯ [0m[31mcannot build native dependency[0m  [31mreason[0m=prebuild-install failed with error and build from sources not possible because platform or arch not compatible
                                    [31mcause[0m=exit status 1
                                    [31merrorOut[0m=prebuild-install info begin Prebuild-install version 7.0.0
    prebuild-install WARN install prebuilt binaries enforced with --force!
    prebuild-install WARN install prebuilt binaries may be out of date!
    prebuild-install info looking for local prebuild @ prebuilds/better-sqlite3-v7.4.5-electron-v89-win32-x64.tar.gz
    prebuild-install info looking for cached prebuild @ /root/.npm/_prebuilds/fa6458-better-sqlite3-v7.4.5-electron-v89-win32-x64.tar.gz
    prebuild-install http request GET https://github.com/JoshuaWise/better-sqlite3/releases/download/v7.4.5/better-sqlite3-v7.4.5-electron-v89-win32-x64.tar.gz
    prebuild-install http request Proxy setup detected (Host: v2ray, Port: 8118, Authentication: No) Tunneling with httpsOverHttp
    prebuild-install WARN install Client network socket disconnected before secure TLS connection was established
    
                                    [31mcommand[0m=/usr/local/bin/node /project/node_modules/prebuild-install/bin.js --platform=win32 --arch=x64 --target=13.4.0 --runtime=electron --verbose --force
                                    [31mworkingDir[0m=/project/node_modules/better-sqlite3
```

解决方法：

下载https://github.com/JoshuaWise/better-sqlite3/releases/download/v7.4.5/better-sqlite3-v7.4.5-electron-v89-win32-x64.tar.gz放到项目根目录的prebuilds文件夹下。



https://www.codenong.com/cs110825785/