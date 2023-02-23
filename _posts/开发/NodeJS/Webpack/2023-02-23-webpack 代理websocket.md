---
tags:
    - NodeJS
    - Webpack
---

webpack 代理websocket

https://github.com/webpack/webpack-dev-server/issues/769



'/ws': {
    target: 'ws://wss_host:wss_port',
    ws: true,
    secure: false
}

