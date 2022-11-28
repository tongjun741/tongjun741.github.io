---
tags:
    - NodeJS
---

```

'/api/': {
      target: apiSite,
      changeOrigin: true,
      onProxyRes: (proxyRes, req, res) => {
        if (proxyRes.statusCode !== 200) {
          console.error(new Date().toLocaleString(), proxyRes.req.path);
        }

        var body = [];
        proxyRes.on('data', function (chunk) {
          body.push(chunk);
        });
        proxyRes.on('end', function () {
          body = Buffer.concat(body).toString();
          try {
            const rs = JSON.parse(body);
            if (rs.code !== 0) {
              throw (new Error(body));
            }
          } catch (e) {
            console.error(new Date().toLocaleString(), e);
          }
        });
      },
    },
```

