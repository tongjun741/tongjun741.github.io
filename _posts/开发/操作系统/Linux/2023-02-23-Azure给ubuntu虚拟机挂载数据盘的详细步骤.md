---
tags:
    - 操作系统
    - Linux
---

Azure给ubuntu虚拟机挂载数据盘的详细步骤

是否选择 SSD 取决于你的使用场景(SSD 还是比较贵的)，默认的大小是 1 T。设置完成后点 "Create" 就可以了。最后保存磁盘配置，就可以登录到系统中查看新添加的磁盘了。

现在登录到系统中查看磁盘情况：

```javascript
$ ls /dev/sd*
/dev/sda  /dev/sda1  /dev/sda14  /dev/sda15  /dev/sdb  /dev/sdb1  /dev/sdc  /dev/sdc1

```

/dev/sdc 就是新磁盘。

查看一下当前系统中的磁盘及挂载情况：

```javascript
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            2.0G     0  2.0G   0% /dev
tmpfs           393M  688K  392M   1% /run
/dev/sda1        29G  8.8G   21G  31% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           2.0G     0  2.0G   0% /sys/fs/cgroup
/dev/sda15      105M  3.6M  101M   4% /boot/efi
/dev/sdc1      1006G   75G  881G   8% /opt
/dev/sdb1        20G   45M   19G   1% /mnt
tmpfs           393M     0  393M   0% /run/user/1000

```



默认情况下，OS 磁盘标记为“/dev/sda”。分区名称为 /dev/sda1，挂载点为 /。

临时磁盘标记为“/dev/sdb”。分区名称为 /dev/sdb1，挂载点为 /mnt。

下面我们就对新添加的磁盘分区并挂载到系统中。

挂载数据磁盘

先使用 fdisk 命令对磁盘进行分区：

```javascript
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc

```

然后使用 mkfs 命令将文件系统写入分区：

```javascript
sudo mkfs -t ext4 /dev/sdc1

```

最后把新的磁盘分区挂载到 /mydata 装载新磁盘使其在操作系统中可访问：

```javascript
sudo mkdir /mydata && sudo mount /dev/sdc1 /opt

```

再使用 df 命令查看结果：

```javascript
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            7.9G     0  7.9G   0% /dev
tmpfs           1.6G  704K  1.6G   1% /run
/dev/sda1        29G  1.2G   28G   5% /
tmpfs           7.9G     0  7.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           7.9G     0  7.9G   0% /sys/fs/cgroup
/dev/sda15      105M  3.6M  101M   4% /boot/efi
/dev/sdb1        63G   53M   60G   1% /mnt
tmpfs           1.6G     0  1.6G   0% /run/user/1000
/dev/sdc1      1006G   77M  955G   1% /opt

```

磁盘分区已经挂载到了目录 /opt。

最后设置开机时挂载

使用 blkid 实用工具获取磁盘的 UUID：

```javascript
$ sudo -i blkid

```

输出的内容类似下面：

```javascript
/dev/sda1: LABEL="cloudimg-rootfs" UUID="431d472c-16c2-48f3-88fb-d6e349d2407b" TYPE="ext4" PARTUUID="f46bd55f-17c3-48a1-9d08-08a61d5221c9"
/dev/sda15: LABEL="UEFI" UUID="70B2-B3D4" TYPE="vfat" PARTUUID="9cc5092d-a05c-441c-958a-f89935f2819e"
/dev/sdb1: UUID="37343997-f572-4853-a656-a78b71eaeae5" TYPE="ext4" PARTUUID="7e3886ad-01"
/dev/sda14: PARTUUID="358dcc8b-5cd2-4f56-a85b-8f76a1321f12"
/dev/sdc1: UUID="b2127ce4-5495-40a3-9fc5-4bb719dba9a9" TYPE="ext4" PARTUUID="d28168f2-01"

```



在 /etc/fstab 文件中添加类似于以下内容的行：

```javascript
UUID="b2127ce4-5495-40a3-9fc5-4bb719dba9a9" /opt ext4 defaults,nofail,barrier=0 1 2

```



保存文件就大功告成了，以后再开机时就会自动完成磁盘的挂载。到这里我们已经完成了添加数据磁盘的所有配置。



https://www.jb51.net/article/131521.htm

