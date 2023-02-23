---
tags:
    - NAS
---



最近上了一个OneDrive for Business的车，拥有5T的OneDrive云盘空间，正好用来备份群晖内的部分重要数据。 [Cloud Sync](https://www.synology.com/zh-cn/knowledgebase/DSM/help/CloudSync/cloudsync) 和 [Hyper Backup](https://www.synology.cn/zh-cn/dsm/feature/hyper_backup) 还是有区别的，Cloud Sync 更多的是做文件同步，Hyper Backup才是真正备份的工具。

然而准备开始使用的时候才发现，Hyper Backup 不能提供将 OneDrive 用作备份目标：![QQ202001051239432x.png](/img-post/开发/NAS/通过Synology的Hyper Backup备份到Microsoft OneDrive for Business（SharePoint）.assets/1578199213.png)

Google Drive 虽可用但是无限空间的账号不知道什么时候会翻车，最终还是放弃选择Google Drive。

最后在群晖的论坛发现有网友实现了将 OneDrive作为备份目标的方法：https://rays-blog.de/2019/07/30/321/backup-to-microsoft-onedrive-for-business-sharepoint-with-synologys-hyper-backup/



原理其实很简单，虽然 Hyper Backup 不支持备份到 OneDrive 但是却支持 [WebDav](https://en.wikipedia.org/wiki/WebDAV),而我们可以通过 WebDAV 访问 OneDrive。不过 OneDrive WebDAV API 需要使用 [Passport服务器端包含（SSI）版本1.4协议进行身份验证](https://docs.microsoft.com/zh-cn/openspecs/windows_protocols/ms-pass/18b675c7-3379-4644-af40-e58c0f56b621)，而 Hyper Backup 的 WebDAV 客户端仅支持 [HTTP基本身份验证](https://en.wikipedia.org/wiki/Basic_access_authentication) 和 [NTLM身份验证](https://en.wikipedia.org/wiki/NT_LAN_Manager)。

不过最终该网友提出了一种解决方案，通过设置代理服务器，将所有与身份验证有关的信息从基本身份验证转换为Passport身份验证（反之亦然），并将所有其他信息不变地转发：[basic-to-passport-auth-http-proxy](https://github.com/skleeschulte/basic-to-passport-auth-http-proxy)

### 设置代理服务器：

1. 通过群晖内 Docker 实现代理服务器的安装： 运行群晖内的Docker套件，找到`映像` - `新增` - `从URL添加` - 填入：https://hub.docker.com/r/skleeschulte/basic-to-sharepoint-auth-http-proxy![QQ202001051258542x.png](/img-post/开发/NAS/通过Synology的Hyper Backup备份到Microsoft OneDrive for Business（SharePoint）.assets/1578200403.png)
2. 启动代理服务器镜像： 下载好镜像后，启动该镜像，选择`高级设置`:

- 勾选`启用自动重新启动`,防止容器异常终止后无法运行

- 容器端口默认是`3000`不要修改，本地端口是映射出来的端口，需要设置一个固定端口后面用

  ![image-20201029211629744](/img-post/开发/NAS/通过Synology的Hyper Backup备份到Microsoft OneDrive for Business（SharePoint）.assets/image-20201029211629744.png)

- 在`环境`选项卡新增变量`PROXY_TARGET`为：https://xxxxxxx-my.sharepoint.com/ （需要登录网页版OneDrive查看浏览器地址栏）点击`应用`并`下一步`可看到该容器的相关配置信息：![image-20201029211827864](/img-post/开发/NAS/通过Synology的Hyper Backup备份到Microsoft OneDrive for Business（SharePoint）.assets/image-20201029211827864.png)

- 再次点击`应用`完成容器的创建，这时候 OneDrive 的代理服务器就设置完成了。

### 设置 Hyper Backup 的备份目标

这时候我们再去 Hyper Backup 新建数据备份任务，选择`WebDAV`，填入对应的参数：

![image-20201029212011465](/img-post/开发/NAS/通过Synology的Hyper Backup备份到Microsoft OneDrive for Business（SharePoint）.assets/image-20201029212011465.png)

- 服务器地址：127.0.0.1:本地端口/personal/用户ID/Documents 本地端口为创建容器时候设置的本地端口，用户ID为网页版地址栏中像用户账号的部分
- 用户账户：OneDrive账户
- 密码：OneDrive密码

填入正确以后，点击`文件夹`下拉列表，应该展示在 OneDrive 中所有文件夹列表。

接下来就是 Hyper Backup 的一系列配置了。

## 参考地址：

 https://rays-blog.de/2019/07/30/321/backup-to-microsoft-onedrive-for-business-sharepoint-with-synologys-hyper-backup/

https://blog.mrabit.com/details/80.html

