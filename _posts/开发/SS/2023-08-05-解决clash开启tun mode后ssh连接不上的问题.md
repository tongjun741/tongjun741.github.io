---
tags:
    - SS
---

现象：

```
$ ssh aaa.com
kex_exchange_identification: Connection closed by remote host
Connection closed by 198.18.0.247 port 22
kex_exchange_identification: Connection closed by remote host
Connection closed by UNKNOWN port 65535
```



配置规则：

```
- DST-PORT,22,DIRECT
```

https://github.com/Fndroid/clash_for_windows_pkg/issues/2139#issuecomment-1079567402

https://lancellc.gitbook.io/clash/clash-config-file/rules/miscellaneous-rule