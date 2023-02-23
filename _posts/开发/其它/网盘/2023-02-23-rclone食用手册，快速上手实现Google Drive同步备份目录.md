---
tags:
    - 其它
    - 网盘
---

rclone是一个高效的linux端文件同步工具，属于Github的开源项目，所以可以放心使用，通过rclone，你可以方便的同步和管理知名网盘。



这次的教程主要是来将如何使用rclone，将linux服务器上的指定目录定时同步至个人用户的Google Drive。

该方法可以配合宝塔的自动备份，可以将宝塔的备份目录同步至Google Drive。

 

下面是详细步骤：

1.首先是更新系统，然后下载安装必要的依赖，并将rclone包放置到正确的位置：

```
yum install unzip wget -y
wget https://downloads.rclone.org/rclone-current-linux-amd64.zip
unzip rclone-current-linux-amd64.zip
cd rclone-*-linux-amd64
cp rclone /usr/bin/
chown root:root /usr/bin/rclone
chmod 755 /usr/bin/rclone
mkdir -p /usr/local/share/man/man1
cp rclone.1 /usr/local/share/man/man1/
mandb
```

2.以上命令分别执行完毕后，就可以开始配置rclone了，

```
rclone config
```

 

3.配置步骤详情（如果你是第一次使用rclone，可以先照搬所有配置）：

首先选n，然后输入配置名称

```
No remotes found - make a new one
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
name> gdrive
```

之后选择储存类型，这次配置的是Google Drive，所以这里选13（随着版本更新，序号会有改动，注意修改）：

```
Type of storage to configure.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / 1Fichier
   \ "fichier"
 2 / Alias for an existing remote
   \ "alias"
 3 / Amazon Drive
   \ "amazon cloud drive"
 4 / Amazon S3 Compliant Storage Provider (AWS, Alibaba, Ceph, Digital Ocean, Dreamhost, IBM COS, Minio, etc)
   \ "s3"
 5 / Backblaze B2
   \ "b2"
 6 / Box
   \ "box"
 7 / Cache a remote
   \ "cache"
 8 / Citrix Sharefile
   \ "sharefile"
 9 / Dropbox
   \ "dropbox"
10 / Encrypt/Decrypt a remote
   \ "crypt"
11 / FTP Connection
   \ "ftp"
12 / Google Cloud Storage (this is not Google Drive)
   \ "google cloud storage"
13 / Google Drive
   \ "drive"
14 / Google Photos
   \ "google photos"
15 / Hubic
   \ "hubic"
16 / In memory object storage system.
   \ "memory"
17 / JottaCloud
   \ "jottacloud"
18 / Koofr
   \ "koofr"
19 / Local Disk
   \ "local"
20 / Mail.ru Cloud
   \ "mailru"
21 / Mega
   \ "mega"
22 / Microsoft Azure Blob Storage
   \ "azureblob"
23 / Microsoft OneDrive
   \ "onedrive"
24 / OpenDrive
   \ "opendrive"
25 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ "swift"
26 / Pcloud
   \ "pcloud"
27 / Put.io
   \ "putio"
28 / QingCloud Object Storage
   \ "qingstor"
29 / SSH/SFTP Connection
   \ "sftp"
30 / Sugarsync
   \ "sugarsync"
31 / Transparently chunk/split large files
   \ "chunker"
32 / Union merges the contents of several remotes
   \ "union"
33 / Webdav
   \ "webdav"
34 / Yandex Disk
   \ "yandex"
35 / http Connection
   \ "http"
36 / premiumize.me
   \ "premiumizeme"
Storage> 13 
```

这里让填写用户ID，这两项不用管，回车直接跳过即可：

```
Google Application Client Id
Setting your own is recommended.
See https://rclone.org/drive/#making-your-own-client-id for how to create your own.
If you leave this blank, it will use an internal key which is low performance.
Enter a string value. Press Enter for the default ("").
client_id> 
Google Application Client Secret
Setting your own is recommended.
Enter a string value. Press Enter for the default ("").
client_secret> 
```

然后是选择访问权限范围，这里选1，最大权限：

```
Scope that rclone should use when requesting access from drive.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / Full access all files, excluding Application Data Folder.
   \ "drive"
 2 / Read-only access to file metadata and file contents.
   \ "drive.readonly"
   / Access to files created by rclone only.
 3 | These are visible in the drive website.
   | File authorization is revoked when the user deauthorizes the app.
   \ "drive.file"
   / Allows read and write access to the Application Data folder.
 4 | This is not visible in the drive website.
   \ "drive.appfolder"
   / Allows read-only access to file metadata but
 5 | does not allow any access to read or download file content.
   \ "drive.metadata.readonly"
scope> 1
```

root文件ID和账户文件这两项也选择回车跳过，不用管：

```
ID of the root folder
Leave blank normally.

Fill in to access "Computers" folders (see docs), or for rclone to use
a non root folder as its starting point.

Note that if this is blank, the first time rclone runs it will fill it
in with the ID of the root folder.

Enter a string value. Press Enter for the default ("").
root_folder_id> 
Service Account Credentials JSON file path 
Leave blank normally.
Needed only if you want use SA instead of interactive login.
Enter a string value. Press Enter for the default ("").
service_account_file> 
```

不进行编辑高级配置，选n：

```
Edit advanced config? (y/n)
y) Yes
n) No (default)
y/n> n
```

远程配置不使用自动配置，选n：

```
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes (default)
n) No
y/n> n
```

接下来会出现一个链接，用于获得Google Drive的授权，这里复制链接，直接通过浏览器访问，同意授权后，会获得一段code，将code粘贴进来，然后回车：

```
Please go to the following link: https://accounts.google.com/o/oauth2/auth?access=dgfdfg6456FuAkQK2Ljw1ECwAg2YO90tUp0ebEDFuAkQK2Ljw1ECw0ebEDFuAkQK2Ljw1ECwYO90tUp0e2LjwLAg2YO654gf56f4g6wAg2YO90tUp0ebEDFuAkQK2Ljw1ECw0ebEDFuAkQK2Ljw1ECw5QjN
Log in and authorize rclone for access
Enter verification code> dfg546dg456fg80ebEDFuAkQK2Ljw1ECw0ebEDFuAkQgb654fg6
```

之后会询问你是否是团队盘，这里我们是个人盘，所以选n：

```
Configure this as a team drive?
y) Yes
n) No (default)
y/n> n
```

这里会生成一次你的配置信息，选y确认：

```
[gdrive]
type = drive
scope = drive
token = {"access_token":"dg354fg.fgd4654dfg4lvtUp0ebEDFuAkQK2LAg2YO90tUp0ebEDFuAkQK2Ljw1ECwAg2YO90tUp0ebEDFuAkQK2Ljw1ECw0ebEDFuAkQK2Ljw1ECw5QjNJ4","token_type":"Bearer","refresh_token":"rrgd6465dg64f6g456g4d64564dfg56gf5J2JIr38rgck0EAg2YO90tUp0ebEDFuAkQK2Ljw1ECw","expiry":"2020-04-23T07:00:465.155.58"}
--------------------
y) Yes this is OK (default)
e) Edit this remote
d) Delete this remote
y/e/d> y
```

到这已经配置完成，这里显示了已有的配置，选q退出：

```
Current remotes:

Name                 Type
====                 ====
gdrive               drive

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q
```

 

4.使用sync命令，进行目录同步（将本地目录/www/backup的内容同步至Google Drive的backup目录，执行时间可以较长，请耐心等待）

```
rclone sync /www/backup gdrive:backup
```

 

5.将同步命令加入定时任务，实现自动同步，首先crontab -e，然后将命令加入最后一行（每周一的8点30进行一次同步）

```
30 8 * * 1 rclone sync /www/backup gdrive:backup
```

 

6.到这里如何将linux本地目录同步至Google Drive的教程就已经完成了，当然rclone的作用并不止这些，下面列出了些常用的命令：

```
rclone copy 复制
rclone move    移动
rclone delete  删除
rclone sync    同步
rclone sync gdrive:backup /home/backup 将配置名为gdrive的Google Drive目录（backup）文件，同步至本地目录（backup）中
rclone sync gdrive2:test gdrive1:backup 将配置名为gdrive2的Google Drive目录（test）文件，同步至配置名为gdrive1的backup目录（backup）中
-p            附加参数：显示实时速度
--bwlimit 40M 附加参数：限制速度40MB
--transfers=N 附加参数：并行文件数
systemctl start rclone       启动rclone
systemctl stop rclone        停止rclone
systemctl status rclone     查看rclone状态
rclone config file          查看配置文件位置
```

##### 本作品采用 [知识共享署名-相同方式共享 4.0 国际许可协议](http://creativecommons.org/licenses/by-sa/4.0/) 进行许可



https://vilark.com/167.html