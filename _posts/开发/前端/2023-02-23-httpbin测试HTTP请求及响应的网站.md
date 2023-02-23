---
tags:
    - 前端
---

# httpbin:测试HTTP请求及响应的网站

来源：https://gitee.com/lyf666/httpbin

https://github.com/postmanlabs/httpbin

| 接口                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [/](http://httpbin.org/)                                     | 主页                                                         |
| [/ip](http://httpbin.org/ip)                                 | 返回原始IP。                                                 |
| [/user-agent](http://httpbin.org/user-agent)                 | 返回用户代理。                                               |
| [/headers](http://httpbin.org/headers)                       | 返回标头字典。                                               |
| [/get](http://httpbin.org/get)                               | 返回GET数据。                                                |
| */post*                                                      | 返回POST数据。                                               |
| */patch*                                                     | 返回PATCH数据。                                              |
| */put*                                                       | 返回PUT数据。                                                |
| */delete*                                                    | 返回DELETE数据                                               |
| [/gzip](http://httpbin.org/gzip)                             | 返回gzip编码的数据。                                         |
| [/deflate](http://httpbin.org/deflate)                       | 返回deflate编码的数据。                                      |
| [/status/:code](http://httpbin.org/status/418)               | 返回给定HTTP状态代码。                                       |
| [/response-headers](http://httpbin.org/response-headers?Content-Type=text/plain; charset=UTF-8&Server=httpbin) | 返回给定的响应标头。                                         |
| [/redirect/:n](http://httpbin.org/redirect/6)                | 302重定向*n*次。                                             |
| [/redirect-to?url=foo](http://httpbin.org/redirect-to?url=http://example.com/) | 302重定向到*foo* URL。                                       |
| [/relative-redirect/:n](http://httpbin.org/relative-redirect/6) | 302相对重定向*n*次。                                         |
| [/cookies](http://httpbin.org/cookies)                       | 返回cookie数据。                                             |
| [/cookies/set?name=value](http://httpbin.org/cookies/set?k1=v1&k2=v2) | 设置一个或多个简单的cookie。                                 |
| [/cookies/delete?name](http://httpbin.org/cookies/delete?k1&k2) | 删除一个或多个简单的cookie。                                 |
| [/basic-auth/:user/:passwd](http://httpbin.org/basic-auth/user/passwd) | 挑战HTTPBasic Auth。                                         |
| [/hidden-basic-auth/:user/:passwd](http://httpbin.org/hidden-basic-auth/user/passwd) | 404’d BasicAuth。                                            |
| [/digest-auth/:qop/:user/:passwd](http://httpbin.org/digest-auth/auth/user/passwd) | 挑战HTTP摘要认证                                             |
| [/stream/:n](http://httpbin.org/stream/20)                   | 流*n* – 100行。                                              |
| [/delay/:n](http://httpbin.org/delay/3)                      | 延迟响应*n* – 10秒。                                         |
| [/drip](http://httpbin.org/drip?numbytes=5&duration=5&code=200) | 在可选的初始延迟之后的一段时间内捕获数据，然后（可选地）返回给定的状态代码。 |
| [/range/:n](http://httpbin.org/range/1024)                   | 流*n*个字节，并允许指定*Range*标头以选择数据的子集。接受*chunk_size*和请求*持续时间*参数。 |
| [/html](http://httpbin.org/html)                             | 呈现HTML页面。                                               |
| [/robots.txt](http://httpbin.org/robots.txt)                 | 返回一些robots.txt规则。                                     |
| [/deny](http://httpbin.org/deny)                             | 被robots.txt文件拒绝。                                       |
| [/cache](http://httpbin.org/cache)                           | 返回200，除非提供If-Modified-Since或If-None-Match标头，否则返回304。 |
| [/cache/:n](http://httpbin.org/cache/60)                     | 为*n*秒设置Cache-Control标头。                               |
| [/bytes/:n](http://httpbin.org/bytes/1024)                   | 生成*n个*随机字节的二进制数据，接受可选的*种子*整数参数。    |
| [/stream-bytes/:n](http://httpbin.org/stream-bytes/1024)     | 流*n个*随机字节的二进制数据，接受可选的*seed*和*chunk_size*整数参数。 |
| [/links/:n](http://httpbin.org/links/10)                     | 返回包含*n个* HTML链接的页面。                               |
| [/forms/post](http://httpbin.org/forms/post)                 | 提交*/发布的* HTML表单                                       |
| [/xml](http://httpbin.org/xml)                               | 返回一些XML                                                  |
| [/encoding/utf8](http://httpbin.org/encoding/utf8)           | 返回包含UTF-8数据的页面。                                    |