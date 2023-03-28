---
tags:
    - NAS
---

## 说明

我使用的设备情况如下：

- 硬件：DS920+
- 系统：DSM 7.1-42661 Update 1
- 套件版本：WebStation 3.1.0-0339

## 必要条件

- 开启DSM的SSH访问功能（通常在**控制面板-终端机和SNMP-启用SSH功能）
- 拥有管理员权限的账户（不一定是root，只要有管理员权限即可，这样可以使用sudo提权）

## 配置步骤

以搭建文件索引服务器为例，演示如何添加自定义配置。

### 使用WebStation创建虚拟机主机

打开WebStation，在网页服务门户栏目中，点击新增。

门户类型选择**虚拟主机**，门户类型**基于名称**，**主机名**填写域名。端口保持默认**80/443**。这一步的配置概要如下。

- 门户类型：基于名称
- 主机名：dl.siamek.com
- 端口：80/443

> 为了演示的简洁，不对HTTPS进行配置。忽略和HTTPS相关的选项。

点完成后击下一步，设置**文档根目录**，这个目录就是需要进行索引的文件夹。**HTTP后段服务器**选择Nginx，其他全部保持默认。这一步的配置概要如下。

- 文档根目录：/web/download
- HTTP后段服务器：Nginx
- 访问控制配置文件：未配置
- 错误页面配置文件：默认错误页面
- 脚本语言设置：未配置

点击下一步，最后一步是超时的设置，全部保持默认即可。最终确认一下所有的配置项，点击**新增**，完成虚拟主机的创建。

> 基于名称的虚拟机需要使用域名进行访问，将dl.simaek.com解析到DSM，测试访问。
>
> 默认主页是**文档根目录**下的index.html，因为演示的是目录索引，所以文档根目录下应该没有主页文件，访问会报404。这说明服务是正常的。如果不放心，可以在**文档根目录**新建一个index.html文件进行测试。

### 添加Nginx自定义配置

这一步需要在终端进行操作。使用SSH命令连接上DSM。

```
ssh user@nas.simaek.com
```

切换到sudo交互模式，避免每次输入命令都要添加sudo的麻烦。这里输入的密码是user用户的密码，user用户必须具有管理员群组权限（可在**控制面板-用户与群组-用户账户**中查看）。

```
sudo -i
Password:
```

切换到Nginx配置文件所在目录，目录下有4个子目录。以**available**结尾的目录中放置的文件并不一定会被启用。当在DSM系统中启用服务的时候，这些文件才会被链接到Nginx真正的配置目录并生效。并且这些配置文件都是以UUID的方式命名的。因此为了便于查看，我们直接操作链接文件即可。链接文件的名称更加具有可读性。

```
cd /usr/local/etc/nginx
ls -l
```

- conf.d：这里的文件都是链接到conf.d-available中的配置文件。
- conf.d-available：一些群晖套件的配置，用户自定义配置，通用选项配置等。
- sites-available：WebStation中的虚拟主机、套件服务器门户等配置。
- sites-enabled：这里的文件都是链接到sites-available中的配置文件。

由上可知，我们需要操作的配置文件是位于`/usr/local/etc/nginx/sites-enabled`目录中的`server.webstation-vhost.conf`，这个配置文件包含了所有WebStation中添加的虚拟主机。

使用vim编辑器打开此文件，文本如下：

```
server {
    listen      80;
    listen      [::]:80;

    listen      443 ssl;
    listen      [::]:443 ssl;

    server_name dl.simaek.com ;

    if ( $host !~ "(^dl.simaek.com$)" ) { return 404; }

    include /usr/syno/etc/www/certificate/WebStation_vhost_d29555d8-e3ba-44ff-b82c-dfc1807fef13/cert.conf*;

    include /usr/syno/etc/security-profile/tls-profile/config/vhost_d29555d8-e3ba-44ff-b82c-dfc1807fef13.conf*;

    ssl_prefer_server_ciphers   on;

    location ^~ /.well-known/acme-challenge {
        root /var/lib/letsencrypt;
        default_type text/plain;
    }

    include conf.d/.webstation.error_page.default.conf*;

    include conf.d/.webstation.error_page.default.resource.conf*;
       
    root    "/volume1/web/download";
    index    index.html  index.htm  index.cgi  index.php  index.php5 ;

    include /usr/local/etc/nginx/conf.d/d29555d8-e3ba-44ff-b82c-dfc1807fef13/user.conf*;

}
```

配置的关键就在于这一Server段配置的最后一行。引入**user.conf**为前缀的文件。默认情况下，此文件不会自动创建，但是此文件的父目录是存在的，我们需要自己创建这个文件。

```
cd /usr/local/etc/nginx/conf.d/d29555d8-e3ba-44ff-b82c-dfc1807fef13
touch user.conf
```

使用vim编辑器打开新创建的**user.conf**文件，填入以下内容。

```
location / {
    autoindex on;
    autoindex_exact_size off;
    autoindex_localtime on;
}
```

> Tips: 这里简单说明一下Nginx索引配置的参数：
>
> `autoindex on|off`是否启用目录索引功能，默认off。
>
> `autoindex_exact_size on|off`是否显示文件的精确大小，默认off（即会以KB、MB、GB等单位自动显示方便阅读的大小）。
>
> `autoindex_localtime on|off`是否显示文件的时间信息，默认off。

修改完成后，需要重新载入Nginx的配置生效，可以在WebStation中先停用虚拟主机再开启。另一种方式是使用Nginx的reload信号。

```
nginx -s reload
```

### 后续说明

自定义修改Nginx的配置，如果你不了解群晖的设计，也许会在**server.webstation-vhost.conf**上直接修改，这也是有效的。但是当你在WebStaion中进行配置变更的时候，所有的修改都会丢失。因为此配置文件是DSM动态生成的。而最后include包含的**user.conf**并不会因此受到影响。

Apache作为后端服务器也是类似的处理方式。配置文件目录位于`/usr/local/etc/apache{version}`，{version}表示Apache的版本号，例如2.4版为`/usr/local/etc/apache24`。



https://www.simaek.com/archives/298/