---
tags:
    - 其它
    - Nginx
---

我们浏览器里的缓存是以 URL 为 key 的，一个 URL 对应一个缓存。但如果浏览器访问了两个 URL 相同但 CORS 响应头不应该相同的资源（即条件型 CORS 响应）会错乱。

比如在同一个浏览器下，先打开了`foo.taobao.com`上的一个页面，访问了我们的资源，这个资源被浏览器缓存了下来，和资源内容一起缓存的还有`Access-Control-Allow-Origin: https://foo.taobao.com`响应头。这时又打开 `bar.taobao.com`上的一个页面，这个页面也要访问那个资源，这时它会读取本地缓存，读到的 `Access-Control-Allow-Origin`头是缓存下的 `https://foo.taobao.com` 而不是自己想要的 `https://bar.taobao.com`，这时就报跨域错误了，虽然它应该是能访问到这份资源的。

## **使用 Vary: Origin 让同一个 URL 有多份缓存**

有一个 HTTP 响应头叫`Vary`，vary 这个单词的意思是“变化”、“不同”的意思，`Vary`响应头就是让同一个 URL 根据某个请求头的不同而使用不同的缓存。比如常见的`Vary: Accept-Encoding`表示客户端要根据`Accept-Encoding`请求头的不同而使用不同的缓存，比如 gizp 的缓存一份，未压缩的缓存为另一份。

在 CORS 的场景下，我们需要使用`Vary: Origin`来保证不同网站发起的请求使用各自的缓存。比如从`foo.taobao.com`发起的请求缓存下的响应头是：

```text
Access-Control-Allow-Origin: https://foo.taobao.com
Vary: Origin
```

的话，`bar.taobao.com`在发起同 URL 的请求就不会使用这份缓存了，因为`Origin`请求头变了。还有`<img>`标签发起的非 CORS 请求缓存下的响应头是：

```text
Vary: Origin
```

的话， 在使用 XHR 发起的 CORS 请求也不会使用那份缓存，因为`Origin`请求头从无到有，也算是变了。



https://zhuanlan.zhihu.com/p/38972475