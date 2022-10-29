---
tags:
    - NodeJS
    - 自动测试
---

Windows Server 2008 R2 下配置uirecorder

关闭IE ESC:

![](../../../_resources/4d5ce8e2e6304099b5db40ace45105c9.png)



配置好代理，安装chrome



安装nodejs 8.9.4：

 https://nodejs.org/dist/v8.9.4/node-v8.9.4-x64.msi



安装java环境：

https://www.java.com/zh_CN/download/windows-64bit.jsp



打开PowerShell：

npm install cnpm -g

cnpm install uirecorder mocha -g

cd C:\

mkdir uirecorder

cd uirecorder

uirecorder init #测试浏览器输入chrome



开始录制：

打开PowerShell：

cd C:\

cd uirecorder

uirecorder

测试脚本文件名：输入脚本保存的位置

打开同步校验浏览器：直接回车

浏览器大小：输入"1280 x 800"

![](../../../_resources/e4761cf80450451597e1578a6d47249d.png)



回放脚本：

打开PowerShell：

cd C:\uirecorder

npm run server

新开一个PowerShell：

cd C:\uirecorder

./run.bat [脚本路径]

![](../../../_resources/f2928054ea304fbe96ee9be90b2a7453.png)









