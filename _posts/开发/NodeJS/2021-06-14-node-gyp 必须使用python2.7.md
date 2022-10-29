---
tags:
    - NodeJS
---

node-gyp 必须使用python2.7

npm install 失败：

MSBUILD : error MSB4132: 无法识别工具版本“2.0”。可用的工具版本为 "4.0"





需要以管理员身份运行：

```javascript
npm install --global --production windows-build-tools

```

或者：

```javascript
npm config set python python2.7
npm config set python "C:\Python27\python.exe"
npm install

```



https://github.com/nodejs/node-gyp#on-windows

