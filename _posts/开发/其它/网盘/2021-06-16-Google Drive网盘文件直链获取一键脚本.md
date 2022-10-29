---
tags:
    - 其它
    - 网盘
---

**说明：**本脚本可以将`Google Drive`网盘的文件分享链接或者文件`ID`变成直链，方便我们在很多情况下调用。只支持文件分享，不支持文件夹。文件分享`ID`为`26`到`48`位。

## 使用

**1、需求**

```
wget、grep、cat、head  #系统一般都有，Centos 7可能要安装wget
```

**2、下载脚本**

```
wget --no-check-certificate -qO /usr/local/bin/gdlink 'https://www.moerats.com/usr/shell/gdlink.sh' && chmod a+x /usr/local/bin/gdlink
```

**3、使用方法**
注意: 获取的分享链接权限为”知道链接的任何人“。

```
#Work with share link/使用分享链接方式
gdlink 'https://drive.google.com/open?id=0B8SvBXZ3I5QMcUduTMJEanRkMzQ'

#Work with file id/使用文件ID方式
gdlink '0B8SvBXZ3I5QMcUduTMJEanRkMzQ'
 
#download with share link/使用分享链接方式直接使用wget下载链接
##可将其中./download改成自己需要的文件名或文件绝对路径
gdlink 'https://drive.google.com/open?id=0B8SvBXZ3I5QMcUduTMJEanRkMzQ' |xargs -n1 wget -c -O ./download
```

**4、调用场景**
比如该`DD`教程：[Linux VPS无限制一键全自动DD安装Windows脚本](https://www.moerats.com/archives/421/)。

先获取到谷歌网盘里的`DD`镜像链接

```
https://drive.google.com/open?id=0B8SvBXZ3I5QMcUduTMJEanRkMzQ
```

调用该分享链接。(将文件`ID`替换为自己的即可)

```
#Work with share link/使用分享链接方式
bash DebianNET.sh -dd "$(echo "https://drive.google.com/open?id=0B8SvBXZ3I5QMcUduTMJEanRkMzQ" |xargs -n1 bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/gdlink.sh'))"
 
#Work with file id/使用文件ID方式
bash DebianNET.sh -dd "$(echo "0B8SvBXZ3I5QMcUduTMJEanRkMzQ" |xargs -n1 bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/gdlink.sh'))"
```

**文章来源：**[[无限制大小\]获取谷歌网盘文件临时直接下载链接](https://moeclub.org/2018/04/01/600/)

------

> 版权声明：本文为原创文章，版权归 [Rat's Blog](https://www.moerats.com/) 所有，转载请注明出处！
>
> 本文链接：https://www.moerats.com/archives/571/
>
> 如教程需要更新，或者相关链接出现404，可以在文章下面评论留言。

`Vultr`新用户注册送`100`美元/`16`个机房按小时计费，支持支付宝，【[点击查看](https://www.moerats.com/archives/543/)】。

 最后修改：2019 年 03 月 12 日 10 : 01 PM

© 著作权归作者所有