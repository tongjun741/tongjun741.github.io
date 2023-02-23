---
tags:
    - 操作系统
    - Windows
---

把 Nginx 创建为 Windows 的一个服务

 把 Nginx 创建为 Windows 的一个服务(一个较好的做法)

        多亏了一个叫做 "Windows Service Wrapper" 的小项目，我们有了一个办法来恰当地启动和停止 Nginx。首先从http://download.java.net/maven/2/com/sun/winsw/winsw/ 下载最新的 exe 程序(Misterdai 写本文时最新的是 "winsw-1.8-bin.exe"。译者已经上传了一个 winsw-1.8-bin.exe 到 CSDN 资源，下载地址：http://download.csdn.net/detail/defonds/4517957)。

        得到该程序后，将其放在 Nginx 安装目录下，并重命名为 myapp.exe。

        然后是告诉 WinSw 我们想要它做什么。这将使用一个 XML 配置文件，我们将在文件中指出 Nginx 需要一个 shutdown 命令。

        (在 Nginx 安装目录下)新建一个名为 myapp.xml 的文件，编辑其内容如下：



<service>  

 <id>nginx</id>  

 <name>nginx</name>  

 <description>nginx</description>  

 <executable>D:\dev\nginx-1.10.2\nginx.exe</executable>  

 <logpath>D:\dev\nginx-1.10.2\</logpath>  

 <logmode>roll</logmode>  

 <depend></depend>  

 <startargument>-p D:\dev\nginx-1.10.2</startargument>  

 <stopargument>-p D:\dev\nginx-1.10.2 -s stop</stopargument>  

</service>  





        很明显，你应该稍微更改文件，这取决于你自己的文件路径。对于有更多技术需求的朋友，你也可以在该文件中设置 Nginx 依赖的服务。

        最后，我们要安装服务了。只需要简单地执行以下语句，你将在你的服务列表里找到 "Nginx" 服务：

[plain] view plain copy

 print?

1. c:\nginx\myapp.exe install  



        就这些！

