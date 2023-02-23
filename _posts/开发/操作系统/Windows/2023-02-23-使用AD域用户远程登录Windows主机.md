---
tags:
    - 操作系统
    - Windows
---

搭建参考：

按https://help.aliyun.com/document_detail/52565.html#h2-url-2中配置完成后可以通过VNC登录域用户，但AD域用户如果不是administrators组的用户无法登录远程桌面，需要按https://blog.csdn.net/bearcatfly/article/details/110432570进行设置



核心：

1、被控主机的DNS设置为域控服务器的IP；

2、将被控主机由workspace改为域；

3、在被控主机远程桌面设置中设置域用户/组可以使用远程桌面。





# ECS实例搭建Windows系统AD域

更新时间：2020-08-10 08:17

[我的收藏](https://help.aliyun.com/my_favorites.html)

[本页目录](javascript:void(0))

- [前提条件](https://help.aliyun.com/document_detail/52565.html#h2-url-1)
- [背景信息](https://help.aliyun.com/document_detail/52565.html#h2-url-2)
- [注意事项](https://help.aliyun.com/document_detail/52565.html#title-w9w-1g2-0fg)
- [操作步骤](https://help.aliyun.com/document_detail/52565.html#title-zmp-7vn-rnh)
- [步骤一：安装AD域控制器](https://help.aliyun.com/document_detail/52565.html#title-te3-nwk-k7u)
- [步骤二：修改客户端的SID](https://help.aliyun.com/document_detail/52565.html#title-4no-2sg-wz1)
- [步骤三：客户端加入AD域](https://help.aliyun.com/document_detail/52565.html#title-lr5-n7q-qpm)
- [相关文档](https://help.aliyun.com/document_detail/52565.html#h2-url-3)

活动目录AD（Active Directory）是微软服务的核心组件。AD能实现高效管理，例如批量管理用户、部署应用和更新补丁等。许多微软组件（例如Exchange）和故障转移群集也需要AD域环境。本文以Windows Server 2012 R2 Datacenter操作系统为例，介绍如何搭建AD域。

## 前提条件

- 安装者必须拥有管理员权限。

- 安装分区必须为NTFS分区。

- 实例必须支持DNS服务。

- 实例必须支持TCP/IP协议，并且需要有固定私网IP地址。

  **说明** 建议使用固定IP地址，防止重启实例后IP地址发生变化。本文采用的是专有网络VPC，手动修改IP地址会导致IP失效。

## 背景信息

- 名词解释：
  - DC：Domain Controllers，域控制器
  - DN：Distinguished Name，识别名
  - OU：Organizational Unit，组织单位
  - CN：Canonical Name，正式名称
  - SID：Security Identifier，安全标识符
- 实例组网信息：
  - 网络类型：专有网络VPC
  - 交换机私网网段：192.168.100.0/24
  - 是否启用网关：是[![VPC组网](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13272.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9268107951/p13272.png)
- 域名信息：
  - 域名：lyonz.com
  - DC：192.168.100.105
  - 需要加入域的客户端（Client）IP地址：192.168.100.106[![域名信息](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13273.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9268107951/p13273.png)

## 注意事项

- 关于域控：
  - 如果您使用自定义镜像创建ECS实例部署域控制器，必须按照本文中的步骤修改SID。如果您使用公共镜像，已经默认修改了SID，无需手动修改。
  - 阿里云不推荐您使用已有的域控制器创建自定义镜像来部署新的域控。如果必须使用，请注意新建实例的主机名（hostname）和创建自定义镜像之前实例的主机名必须保持一致，否则可能会报错`服务器上的安全数据库没有此工作站信任关系`；您也可以在创建实例后修改成相同的主机名，解决此问题。
- 关于客户端：阿里云不推荐您使用已加入域的客户端实例来创建自定义镜像，否则新镜像创建的实例会报错`服务器上的安全数据库没有此工作站信任关系`。如果确实需要，建议您在创建新的自定义镜像前先退域。

## 操作步骤

通过Windows Server 2012实例搭建AD域的步骤如下：

1. [步骤一：安装AD域控制器](https://help.aliyun.com/document_detail/52565.html#section-vdo-3hc-rfl)
2. [步骤二：修改客户端的SID](https://help.aliyun.com/document_detail/52565.html#section-xex-k3r-hup)
3. [步骤三：客户端加入AD域](https://help.aliyun.com/document_detail/52565.html#section-990-tcm-nuy)

## 步骤一：安装AD域控制器

1. 远程连接ECS实例。连接方式请参见[连接方式概述](https://help.aliyun.com/document_detail/71529.htm#concept-tmr-pgx-wdb)。

2. 打开**服务器管理器**，在ECS实例中添加角色和功能。

   [![添加角色和功能](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13276.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9268107951/p13276.png)

   1. 选择安装类型。

      [![选择安装类型](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13277.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9268107951/p13277.png)

   2. 选择要安装角色和功能的服务器。

      [![选择要安装角色和功能的服务器](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13278.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9268107951/p13278.png)

   3. 选择要安装在服务器上的角色。

      [![选择要安装在服务器上的角色](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13279.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9268107951/p13279.png)

3. 将此服务器提升为域服务器。

   [![将此服务器提升为域服务器](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13280.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9268107951/p13280.png)

   1. 部署配置。

      [![部署配置](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13281.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9268107951/p13281.png)

   2. 配置域服务器参数。

      [![配置域服务器参数](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13282.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9268107951/p13282.png)

   3. 配置DNS选项。

      [![配置DNS选项](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13283.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9268107951/p13283.png)

   4. 配置NetBIOS域名。

      [![配置NetBIOS域名](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13284.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13284.png)

   5. 检查并确认您的选择。

      [![检查并确认您的选择](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13285.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13285.png)

   6. 单击**下一步**。

   7. 单击**安装**，开始安装AD域服务器。

      [![开始安装AD域服务器](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13286.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13286.png)

      安装完成，如下图所示。[![安装完成，如下图所示](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13287.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13287.png)

## 步骤二：修改客户端的SID

1. 下载修改客户端SID的PowerShell脚本。

   - 下载地址：[AutoSysprep.ps1](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/40846/cn_zh/1542010494209/AutoSysprep.ps1)
   - 脚本来源：阿里云官方

2. 打开CMD，输入powershell切换至**Windows PowerShell**界面。

   **说明** 如果您的实例操作系统是64位，则不能使用32位的PowerShell（即**Windows PowerShell (x86)**），否则会报错。

3. 切换至脚本存储的路径，执行如下命令，查看脚本工具说明。

   ```
   .\AutoSysprep.ps1 -help
   ```

4. 执行如下命令，服务器会重新初始化SID。

   ```
   .\AutoSysprep.ps1 -ReserveHostname -ReserveNetwork -SkipRearm -PostAction "reboot"
   ```

   初始化完成后，会重启实例，您需要注意以下事项。

   - 实例IP地址会从DHCP变成固定IP地址。您可以重新改成DHCP，请参见[修改私有IP地址](https://help.aliyun.com/document_detail/27733.htm#task-bng-thn-xdb)。[![重新改成DHCP](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13288.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13288.png)
   - 初始化SID后，云服务器防火墙的配置被修改成微软的默认配置，导致云服务器无法Ping通。您需要关闭防火墙**来宾或公用网络**，或者放行需要开放的端口。下图表示防火墙**来宾或公用网络**的状态是已连接。[![来宾或公用网络](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13291.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13291.png)

5. 关闭来宾或公用网络防火墙。

   [![关闭来宾或公用网络防火墙](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13292.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13292.png)

   关闭后，可以Ping通服务器。[![PING通服务器示意图](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13293.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13293.png)

## 步骤三：客户端加入AD域

您可以根据业务需求修改主机名和DNS指向DC的IP地址。

1. 修改DNS服务器地址。

   [![修改DNS服务器地址](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13294.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13294.png)

2. 检查是否能Ping通DNS服务器IP地址。

   [![PING通DNS服务器的地址](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13295.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13295.png)

3. 修改主机名并加入AD域。

   [![修改主机名并加入AD域](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/p13296.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0368107951/p13296.png)





## 问题：windows客户端加入AD域后，远程桌面无法登录。报错误：连接被拒绝，因为没有授权此用户帐户进行远程登录

![img](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/20201201145147593.png)

解决方案：

直接进入客户端，不通过远程方式。登入后，进入远程桌面管理，选择用户，添加用户即可远程登录。

![img](/img-post/开发/操作系统/Windows/使用AD域用户远程登录Windows主机.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JlYXJjYXRmbHk=,size_16,color_FFFFFF,t_70.png)