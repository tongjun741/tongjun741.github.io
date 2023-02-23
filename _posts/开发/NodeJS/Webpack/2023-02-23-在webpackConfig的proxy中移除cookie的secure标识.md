---
tags:
    - NodeJS
    - Webpack
---

在webpackConfig的proxy中移除cookie的secure标识

在webpackConfig的proxy中移除cookie的secure标识：



```javascript
onProxyRes: (proxyRes, req, res) => {
  const sc = proxyRes.headers['set-cookie'];
  if (Array.isArray(sc)) {
    proxyRes.headers['set-cookie'] = sc.map(sc => {
      return sc.split(';')
        .filter(v => v.trim().toLowerCase() !== 'secure')
        .join('; ')
    });
  }
},

```



效果：

```javascript
proxy: {
    '/api/*': {
        target: 'http'+(backendServerUseHttps?'s':'')+'://'+backendServerHost+':' + backendServerPort + '/',
        changeOrigin: true,
        secure: false,
        onProxyRes: (proxyRes, req, res) => {
            const sc = proxyRes.headers['set-cookie'];
            if (Array.isArray(sc)) {
                proxyRes.headers['set-cookie'] = sc.map(sc => {
                    return sc.split(';')
                        .filter(v => v.trim().toLowerCase() !== 'secure')
                        .join('; ')
                });
            }
        }
    },

    '/portal_socketio':{
        target: 'ws'+(backendServerUseHttps?'s':'')+'://' + backendServerHost+':'+ portalWebSocketPort +'/portal_socketio',
        pathRewrite: {'^/portal_socketio': ''},
        changeOrigin: true,
        ws: true,
        secure: false
    },
    '/aa/oldjs/*': 'http://127.0.0.1:' + backendServerPort + '/',
    '/bb/thirdparty_libs/*': 'http://127.0.0.1:' + backendServerPort + '/',
}

```

https://github.com/http-party/node-http-proxy/issues/1165#issuecomment-431025061

