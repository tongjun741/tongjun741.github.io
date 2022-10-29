---
tags:
    - 操作系统
    - Linux
---

linux中利用curl获得http请求的响应时间

curl -o j_out -s www.baidu.com -w "%{time_total}\n"


https://blog.csdn.net/rogerjava/article/details/18415263