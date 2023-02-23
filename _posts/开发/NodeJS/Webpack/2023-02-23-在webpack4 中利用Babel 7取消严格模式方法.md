---
tags:
    - NodeJS
    - Webpack
---

在webpack4 中利用Babel 7取消严格模式方法

报错信息：

Uncaught TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them

 

出现个个问题的原因，是在引用mui中的mui.js文件时报的错误，由于在webpack中，采用的时严格模式，而mui.js用了非严格模式的写法。

解决方法：1.把mui.js里面的内容改成严格模式，但这不可能，毕竟我们要引用他们。

　　　　　2. 只能把webpack改为非严格模式了

 

安装：

cnpm i @babel/plugin-transform-modules-commonjs @babel/plugin-transform-strict-mode -D

 

在.babelrc中进行配置：

"plugins": [
      ["@babel/plugin-transform-modules-commonjs", { "strictMode": false }]
    ]

这样就解决了。0.0

 

https://www.cnblogs.com/zengsf/p/10981537.html

