---
tags:
    - 操作系统
    - Linux
---

config/supervisor/xxx.ini

```
[program:xxx]
command=python /aa/bb/xxx.py
directory=/shareVolume/config/xxx
stdout_logfile=/shareVolume/config/xxx/log/stdout.log
stderr_logfile=/shareVolume/config/xxx/log/stderr.log
autostart=true
autorestart=true
priority=40
startsecs=15
stdout_logfile_maxbytes=5GB
```



https://www.jianshu.com/p/7e788634257b