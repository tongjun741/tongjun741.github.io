---
tags:
    - 操作系统
    - Linux
---

Centos 7 搭建rsyslog服务



```javascript
一、搭建并测试和运行rsyslog
1、centos7上安装rsyslog
yum  install  rsyslog  -y
2、关闭防火墙
systemctl  stop firewalld
3、配置rsyslog配置文件/etc/rsyslog.conf，修改以下内容
取消注释：
$ModLoad imklog   //内核日志功能
$ModLoad immark   //日志标签功能
$ModLoad imudp   //启用UDP接收syslog , 一般设备syslog都是走UDP
$UDPServerRun 514   //端口号514
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat
$ActionFileEnableSync on
底部增加配置：
$template IpTemplate,"/var/log/syslog/%FROMHOST-IP%/%$year%-%$month%-%$day%.log.log"
*.*  ?IpTemplate
& ~
第一行是定义了一个日志接收存储模板，名称为IpTemplate（可任意）,"/var/log/syslog/%FROMHOST-IP%/%$year%-%$month%-%$day%.log.log"是日志文件存储路径，用到了几个变量，根据不同IP分了目录，每个IP目录下每天会生成一个日志文件
第二行是对任意类型调用这个名为IpTemplate的模板
4、配置完成，启动rsyslog服务
systemctl  restart  rsyslog
5、配置好log源端设备后监控日志记录情况，例如：
tail -f /var/log/syslog/192.168.200.X/2018-10-20.log.log


logger -T -P 514 -n 192.168.1.204 'Hello,World!'

```



http://blog.sina.com.cn/s/blog_4c86552f0102y11t.html



https://blog.csdn.net/yiifaa/article/details/76735302

