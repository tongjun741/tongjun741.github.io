---
tags:
    - NodeJS
---

Mac 下 npm install deasync出错



```javascript

> deasync@0.1.14 install /Users/node_modules/deasync
> node ./build.js

gyp ERR! configure error
gyp ERR! stack Error: Command failed: /usr/local/bin/python -c import sys; print "%s.%s.%s" % sys.version_info[:3];
gyp ERR! stack   File "<string>", line 1
gyp ERR! stack     import sys; print "%s.%s.%s" % sys.version_info[:3];
gyp ERR! stack                                ^
gyp ERR! stack SyntaxError: invalid syntax
gyp ERR! stack
gyp ERR! stack     at ChildProcess.exithandler (child_process.js:290:12)
gyp ERR! stack     at ChildProcess.emit (events.js:200:13)
gyp ERR! stack     at maybeClose (internal/child_process.js:1021:16)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:283:5)
gyp ERR! System Darwin 16.3.0
gyp ERR! command "/usr/local/Cellar/node/12.4.0/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Users/node_modules/deasync
gyp ERR! node -v v12.4.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok
Build failed

```



原因是系统中没有python2，使用以下命令安装python2就好了：

```javascript
brew install python@2

```



