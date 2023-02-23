---
tags:
    - 操作系统
    - Linux
---

curl 查看url的httpCode


```
curl -I -m 10 -o /dev/null -s -w %{http_code} www.baidu.com
```

Mac 下 "-I"参数要改为 "-i":

```
curl -i -m 10 -o /dev/null -s -w %{http_code} www.baidu.com
```


-I 仅测试HTTP头
-m 10 最多查询10s
-o /dev/null 屏蔽原有输出信息
-s silent
-w %{http_code} 控制额外输出
 
绑定 ip 测试：

```
curl -I -m 10  -H "www.baidu.com"  http://220.181.112.143 -o /dev/null -s -w %{http_code}
```
