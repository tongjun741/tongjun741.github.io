---
tags:
    - NodeJS
---

### 前言

在node环境下做些简单的爬虫时，需要用代理地址，如果访问的目标站点
是https协议，用axios就会有些问题

### 解决方法

1.使用tunnel隧道代理

> node-tunnel - 用于HTTP/HTTPS的隧道代理

具体参考(tunnel)[[https://www.npmjs.com/package...](https://link.segmentfault.com/?enc=0Sgojr9NjkHNFBw8f0huVw%3D%3D.r0tJr0VWiJBXVXL1pfGV7ttg3mUYcfQUSNngBwnLSJnVGs3n4%2F%2FELd%2F7rPzNyBHj)]

- 安装
  `npm install tunnel`
- 使用

```javascript
const axios = require('axios')
const tunnel = require('tunnel')

const tunnelProxy = tunnel.httpsOverHttp({
    proxy: {
        host: 'you_host',
        port: 'you_port',
    },
});

axios(url,{
    proxy: false,
    httpsAgent: tunnelProxy,
    timeout: 10000
})
.then(res=>{
    console.log(res.data)
})
```

2.使用 `axios-https-proxy-fix`

`axios-https-proxy-fix`是对`axios`https请求bug修复的一个分支版本

- 安装
  `npm i axios-https-proxy-fix`
- 使用

```arcade
const axios = require('axios-https-proxy-fix')

axios(url,{
    proxy: {
        host: '127.0.0.1',
        port: '1080'
    },
    timeout: 10000
})
.then(res=>{
    console.log(res.data)
})
```

3.使用node `request`模块

**个人感觉如果只是在服务端运行的代码，用这个最为稳妥**

- 安装
  `npm i request`
- 使用

```moonscript
const request = require('request')

request({
    url,
    timeout: 5000,
    proxy: 'http://127.0.0.1:1080'
},(error,response,body)=>{
    if (error) {
        return    console.log(error)
    }
    console.log(body)
})
```



https://segmentfault.com/a/1190000020008982