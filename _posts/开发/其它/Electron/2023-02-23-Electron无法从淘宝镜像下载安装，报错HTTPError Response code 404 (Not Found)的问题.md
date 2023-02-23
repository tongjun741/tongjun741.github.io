---
tags:
    - 其它
    - Electron
---

# 问题产生

这是一个突然发生的问题。使用 `yarn add --dev electron` 安装 electron 时报错如下：

HTTPError: Response code 404 (Not Found) for [npm.taobao.org/mirrors/ele…](https://link.juejin.cn?target=http%3A%2F%2Fnpm.taobao.org%2Fmirrors%2Felectron%2Fv16.0.0%2Felectron-v16.0.0-win32-x64.zip)

> 查看github，发现 5 小时前，electron更新了 v16 版本的正式版。也包括 v15 版本的更新 v15.3.2

这个问题是因为下载 electron 时，请求的下载路径不存在(404)导致的，也就是安装包 `http://npm.taobao.org/mirrors/electron/v16.0.0/electron-v16.0.0-win32-x64.zip` 不存在。可直接访问 [npm.taobao.org/mirrors/ele…](https://link.juejin.cn?target=http%3A%2F%2Fnpm.taobao.org%2Fmirrors%2Felectron) 查看对应url路径是否存在：

![img](/img-post/开发/其它/Electron/Electron无法从淘宝镜像下载安装，报错HTTPError Response code 404 (Not Found)的问题.assets/68764e9bdcef4d6f8aae40e24d78588f~tplv-k3u1fbpfcp-watermark.awebp)

安装时的完整报错内容信息如下：

```sh
$ yarn add --dev electron
yarn add v1.22.17
info No lockfile found.
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
error D:\SoftWareDevelope\Work\AutoC\node_modules\electron: Command failed.Exit code: 1
Command: node install.js
Arguments:
Directory: D:\SoftWareDevelope\Work\AutoC\node_modules\electron
Output:
HTTPError: Response code 404 (Not Found) for http://npm.taobao.org/mirrors/electron/v16.0.0/electron-v16.0.0-win32-x64.zip
    at EventEmitter.<anonymous> (D:\SoftWareDevelope\Work\AutoC\node_modules\got\source\as-stream.js:35:24)
    at EventEmitter.emit (events.js:376:20)
    at module.exports (D:\SoftWareDevelope\Work\AutoC\node_modules\got\source\get-response.js:22:10)
    at ClientRequest.handleResponse (D:\SoftWareDevelope\Work\AutoC\node_modules\got\source\request-as-event-emitter.js:155:5)
    at Object.onceWrapper (events.js:483:26)
    at ClientRequest.emit (events.js:388:22)
    at ClientRequest.origin.emit (D:\SoftWareDevelope\Work\AutoC\node_modules\@szmarczak\http-timer\source\index.js:37:11)
    at HTTPParser.parserOnIncomingClient [as onIncoming] (_http_client.js:647:27)
    at HTTPParser.parserOnHeadersComplete (_http_common.js:126:17)
    at Socket.socketOnData (_http_client.js:515:22)
info Visit https://yarnpkg.com/en/docs/cli/add for documentation about this command.
复制代码
```

# 问题的原因和解决

这个问题的产生有两个原因：

## taobao镜像未同步最新的版本

- 一是，新更新的 electron 版本在taobao镜像中还未同步过来，也就是 taobao 镜像中没有16版本的electron。

npm的淘宝官方镜像地址 [npmmirror.com/](https://link.juejin.cn?target=https%3A%2F%2Fnpmmirror.com%2F) 中，可以看到有介绍，使用 `cnpm sync` 同步一个模块的方法。尝试使用它同步一下electron，不成功

```sh
cnpm sync electron
复制代码
```

也可能只能同步 npm 下的某个小模块，而安装 electron 时，使用的是单独的electron镜像地址：[npm.taobao.org/mirrors](https://link.juejin.cn?target=http%3A%2F%2Fnpm.taobao.org%2Fmirrors) 。具体原因不知，反正结果是 [npm.taobao.org/mirrors](https://link.juejin.cn?target=http%3A%2F%2Fnpm.taobao.org%2Fmirrors) 中没有同步进来最新的electron。

如果没有同步，那么，只能安装指定的在淘宝镜像中存在的版本。

## 淘宝镜像中的electron的url路径没有版本标识'v'

- 二是，淘宝镜像中的electron实际路径包含的版本标识中，没有 v 字符。

这个问题的对比就是：

electron 安装请求的地址是： [npm.taobao.org/mirrors/ele…](https://link.juejin.cn?target=http%3A%2F%2Fnpm.taobao.org%2Fmirrors%2Felectron%2Fv15.3.1%2F) electron 淘宝镜像的地址是： [npm.taobao.org/mirrors/ele…](https://link.juejin.cn?target=http%3A%2F%2Fnpm.taobao.org%2Fmirrors%2Felectron%2F15.3.1%2F)

此处以 v15.3.1 为例，因为其版本当前在淘宝镜像路径中存在。

也就是 淘宝镜像 url 路径中，没有 v 标识版本，版本号前没有字符 `v`。

解决这个问题的办法就是，修改组成 electron 下载地址的 npm 变量。`@electron/get`(当前项目的 node_modules 下) 使用的 url 由如下组成：

```sh
url = ELECTRON_MIRROR + ELECTRON_CUSTOM_DIR + '/' + ELECTRON_CUSTOM_FILENAME
复制代码
```

`ELECTRON_CUSTOM_DIR` 的默认值为 `v$VERSION`，可以使用 `{{ version }}` 占位符修改默认的格式，`v{{ version }}` 等于默认值，则将其修改为 `{{ version }}` 即可。

直接在shell命令行中，修改npm的配置变量 `ELECTRON_CUSTOM_DIR` 就可以实现正确的版本路径的对应：

如下，直接在命令行中执行 `npm config set` ，修改为正确的版本号。

```sh
npm config set ELECTRON_CUSTOM_DIR "{{ version }}"
复制代码
```

如下，为修改后的操作

```sh
$ npm config set ELECTRON_CUSTOM_DIR "{{ version }}"
$ yarn add --dev electron
yarn add v1.22.17
info No lockfile found.
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
error D:\SoftWareDevelope\Work\Auto\node_modules\electron: Command failed.
Exit code: 1
Command: node install.js
Arguments:
Directory: D:\SoftWareDevelope\Work\Auto\node_modules\electron
Output:
HTTPError: Response code 404 (Not Found) for http://npm.taobao.org/mirrors/electron/16.0.0/electron-v16.0.0-win32-x64.zip
......
复制代码
```

可以看到，请求地址中的路径，已经变为 `electron/16.0.0/` ，但由于未同步过来的原因，仍然会报错（若同步过来则可以正确安装执行）。

## 修改electron下载请求url中的ELECTRON_MIRROR

还可以修改，electron下载请求url中的ELECTRON_MIRROR。比如上面默认使用的是非CDN的淘宝镜像地址。还可以根据需要设置为淘宝镜像的CDN地址：

```sh
npm config set ELECTRON_MIRROR "https://cdn.npm.taobao.org/dist/electron/"
复制代码
```

非CDN的淘宝镜像

```sh
npm config set ELECTRON_MIRROR "https://npm.taobao.org/mirrors/electron/"
复制代码
```

> ELECTRON_CUSTOM_DIR、ELECTRON_MIRROR等本身就是环境变量参数。

# 最终解决：修改 ELECTRON_CUSTOM_DIR 请求的url路径和指定淘宝镜像存在的electron版本

如下，为 修改 ELECTRON_CUSTOM_DIR 请求的url路径和指定淘宝镜像存在的electron版本，进行正确安装的过程：

```sh
$ npm config set ELECTRON_CUSTOM_DIR "{{ version }}"
$ yarn add --dev electron@15.3.1
yarn add v1.22.17
info No lockfile found.
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
info There appears to be trouble with your network connection. Retrying...
success Saved lockfile.
success Saved 78 new dependencies.
info Direct dependencies
└─ electron@15.3.1
info All dependencies
├─ @electron/get@1.13.1
......省略
├─ yallist@4.0.0
└─ yauzl@2.10.0
Done in 26.07s.
复制代码
```

# 附：修改 `@electron/get` 下配置文件的地址【不推荐，仅做参考】

对与url路径中的版本标识字符v 的问题，还可以通过直接修改 `@electron/get` 中的变量实现

找到 `const path = mirrorVar('customDir', opts, details.version).replace('{{ version }}', details.version.replace(/^v/, ''));` 这一行（目前版本为第46行）

将其修改为 `const path = mirrorVar('customDir', opts, details.version.replace(/^v/, '')).replace('{{ version }}', details.version.replace(/^v/, ''));`。

重点是 `details.version.replace(/^v/, '')` 这句替换！

# 参考

- [Electron-Advanced Installation Instructions#mirror](https://link.juejin.cn?target=https%3A%2F%2Fwww.electronjs.org%2Fdocs%2Flatest%2Ftutorial%2Finstallation%23mirror)
- [解决electron安装卡在node install.js和Response 404(Not Found)问题](https://link.juejin.cn?target=https%3A%2F%2Fblog.csdn.net%2Fweixin_44633882%2Farticle%2Fdetails%2F104555932)
- [electron无法从淘宝镜像下载](https://link.juejin.cn?target=https%3A%2F%2Fblog.csdn.net%2Fsouvir%2Farticle%2Fdetails%2F104952859)


作者：代码迷途
链接：https://juejin.cn/post/7033932629128773669
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。