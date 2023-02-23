---
tags:
    - Docker
---

```
sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list
apt-get update
apt-get search jdk
apt-get install openjdk-11-jdk  -y
```

