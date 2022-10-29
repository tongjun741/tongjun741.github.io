---
tags:
    - 其它
    - 网盘
---

## 前言

在日常资料存储中，私密资料（版权或者其它），由于本地存储空间的限制亦或是容灾备份，在云端硬盘供应商存储是一个优选方案。
然而在一些云端硬盘中，含有特定版权或者特定资源时，可能导致账号封禁或者其它影响，当然也有可能账号被盗之类的。所以通过某种加密方案来进行数据存储是大部分人的选择。
当然加密存储的方案很多，比如加密压缩等。

如果你选用的是rclone支持的云端硬盘，那么可以使用更加直接有效的加密方式-rclone crypt。

## 加密说明

rclone的加密和云端硬盘无直接联系，而是当你在上传资料时，在本地对你的资料进行加密，然后再上传到云端硬盘。同理，在下载资料时，先将加密的资料下载到本地，然后再对其进行解密。
这样也就说明，云端硬盘是无法知晓你的任何加密原始数据的。

下面对rclone crypt的使用方式做简要说明。

## 加密配置

首先新建一个加密远端（remote），加密的远端必须是当前已经创建过的远端（remote），比如：Onedrive、Googledrive等等。rclone可以使用已存在远端（remote）的任意路径或者根路径（全盘）来作为加密远端。

1. 命令哈模式下进入配置交互界面

```
rclone config
```

1. 新建一个远端（remote）

```
e) Edit existing remote

n) New remote

d) Delete remote

r) Rename remote

c) Copy remote

s) Set configuration password

q) Quit config

e/n/d/r/c/s/q> n
```

1. 输入远端（remote）名称

```
name>Crypted
```

1. 选择配置存储类型

```
...

10 / Encrypt/Decrypt a remote

   \ "crypt"

...

Storage>10
```

这里可以选择填写序号10，也可以添加类型命令`crypt`，由于rclone支持的存储类型可能不断增加，在填写序号时著注意确认清楚。

1. 设置加密路径

```
** See help for crypt backend at: https://rclone.org/crypt/ **



Remote to encrypt/decrypt.

Normally should contain a ':' and a path, eg "myremote:path/to/dir",

"myremote:bucket" or maybe "myremote:" (not recommended).

Enter a string value. Press Enter for the default ("").

remote>GD:/crypted
```

注意：路径应当存在，可以通过`rclone lsd GD:`来查看是否存在该路径。

1. 选择如何加密文件名

```
How to encrypt the filenames.

Enter a string value. Press Enter for the default ("standard").

Choose a number from below, or type in your own value

 1 / Encrypt the filenames see the docs for the details.

   \ "standard"

 2 / Very simple filename obfuscation.

   \ "obfuscate"

 3 / Don't encrypt the file names.  Adds a ".bin" extension only.

   \ "off"

filename_encryption> 1
```

可以选择标准、简单混淆或者不加密，推荐使用标准加密方式。需要注意的是，标准加密方式会是你的文件名称变得更长，然而有些存储商可能对文件名称长度存在限制，择情选则。

1. 是否加密目录名称

```
Option to either encrypt directory names or leave them intact.

Enter a boolean value (true or false). Press Enter for the default ("true").

Choose a number from below, or type in your own value

 1 / Encrypt directory names.

   \ "true"

 2 / Don't encrypt directory names, leave them intact.

   \ "false"

directory_name_encryption> 1
```

1. 为你的加密设置相关密码

```
Password or pass phrase for encryption.

y) Yes type in my own password

g) Generate random password

y/g> y

Enter the password:

password:

Confirm the password:

password:
```

可以自行设置，也可以自动生成。

1. 第二个密码

```
Password or pass phrase for salt. Optional but recommended.

Should be different to the previous password.

y) Yes type in my own password

g) Generate random password

n) No leave this optional password blank (default)

y/g/n> g

Password strength in bits.

64 is just about memorable

128 is secure

1024 is the maximum

Bits> 1024

Your password is: 

Use this password? Please note that an obscured version of this

password (and not the password itself) will be stored under your

configuration file, so keep this generated password in a safe place.

y) Yes (default)

n) No

y/n> y
```

这个密码可以不设置，不过还是推荐加上。自动生成的密码，记得复制，备份。

1. 加密完成

```
Remote config

--------------------

[Crypted]

type = crypt

remote = GD:/crypted

filename_encryption = standard

directory_name_encryption = true

password = *** ENCRYPTED ***

password2 = *** ENCRYPTED ***
```

到这一步，配置完成。上传在文件在云端正常查看的时候是乱码，只有通过rclone查看这个加密远端才显示正常。

加密远端支持rclone的任意命令，只是在web端或者正常的远端查看是乱码字符。

## 最后提醒，密码一定要备份保存，如果忘记了的话，你存的资源就game over。



https://ikfou.com/archives/287.html