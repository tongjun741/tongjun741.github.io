---
tags:
    - NAS
---

```
sudo docker pull cloudnas/clouddrive2

sudo mount --make-shared /volume2/

sudo docker run -d \
    --name clouddrive \
    --restart unless-stopped \
    --env CLOUDDRIVE_HOME=/Config \
    -v /volume2/docker/webdav/CloudDrive:/CloudNAS:shared \
    -v /volume2/docker/webdav/CloudDrive-config:/Config \
    -v /volume2/docker/webdav/CloudDrive-media:/media:shared \
    --network host \
    --pid host \
    --privileged \
    --device /dev/fuse:/dev/fuse \
    cloudnas/clouddrive2
```



https://www.clouddrive2.com/docker.html