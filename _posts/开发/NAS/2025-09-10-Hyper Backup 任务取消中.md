---
tags:
    - NAS
---

你随时可以用 sudo sync; sudo reboot 或者 sudo sync; sudo synopoweroff重启。别直接拔电源。

或者你也可以只重启 HyperBackup 服务。用 sudo synoservice --status 找到它的名字，然后用 sudo synoservicectl --restart servicename重启它。