---
tags:
    - NodeJS
    - Webpack
---

Webpack proxy 修改html 资源的body

参考网址：

https://github.com/chimurai/http-proxy-middleware

https://github.com/nodejitsu/node-http-proxy/issues/796

https://github.com/nodejitsu/node-http-proxy/issues/796



webpack.config.js


```
const path = require('path');

module.exports = {
    entry: './index.js',
    devServer: {
        host : '0.0.0.0',
        port: 4321,
        contentBase: ".",
        progress: true,
        hot: true,
        inline: true,
        stats: { colors: true },
        noInfo: true,
        historyApiFallback: true,
        // proxy calls to api to our own node server backend
        proxy: {
            '*':{
                target: 'http://127.0.0.1:8080',
                onProxyRes: function(proxyRes, request, response) {
                    let contentLength = 0;
                    delete proxyRes.headers['content-length'];
                    function modifyHtml( str ) {
                        str = str.replace(/20170921092248/g, 'xxx-tj');

                        contentLength = str.length;
                        return str;
                    }

                    if( proxyRes.headers &&
                        proxyRes.headers[ 'content-type' ] &&
                        proxyRes.headers[ 'content-type' ].match( 'text/html' ) ) {

                        var _end = response.end,
                             _write = response.write,
                            chunks,
                            _writeHead = response.writeHead;

                        response.writeHead = function(){
                            // if( proxyRes.headers && proxyRes.headers[ 'content-length' ] ){
                            //     response.setHeader(
                            //         'content-length',
                            //         contentLength
                            //     );
                            // }

                            // This disables chunked encoding
                            response.setHeader( 'transfer-encoding', '' );

                            // Disable cache for all http as well
                            response.setHeader( 'cache-control', 'no-cache' );

                            _writeHead.apply( this, arguments );
                        };

                        response.write = function( data ) {
                            if( chunks ) {
                                chunks += data;
                            } else {
                                chunks = data;
                            }
                        };

                        response.end = function() {
                            if( chunks && chunks.toString )
                                _write.apply( this, [ modifyHtml( chunks.toString() ) ] );
                            _end.apply( this, arguments );
                        };
                    }
                }
            }
        }
    },
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    }
};
```
