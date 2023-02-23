---
tags:
    - 操作系统
    - Linux
---

CentOS 7 安装php-oci8



```javascript
yum -y install epel-release
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
yum install php70w php70w-common php70w-fpm php70w-pdo  php70w-xml php70w-mbstring -y

service httpd start
service firewalld stop

```



https://blog.csdn.net/yy211zhu/article/details/77509441

```javascript
rpm -Uvh  https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-7-11.noarch.rpm
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
yum —enablerepo=remi,remi-php55 install httpd php php-devel libaio gcc -y

yum install php70w-devel php70w-pear libaio gcc -y

```





https://medium.com/@asasmoyo/setup-php-oci8-on-centos-7-ubuntu14-04-b9d97383fda4

```javascript
去 http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html

或者我用的版本https://download.csdn.net/download/wq57885/10511204

下载适合自己的，2018-7-1我下载的是

oracle-instantclient12.2-basic-12.2.0.1.0-1.x86_64.rpm

oracle-instantclient12.2-devel-12.2.0.1.0-1.x86_64.rpm

二：传到linux上，我的是虚拟机

我传到root下，然后执行命令

rpm -ivh oracle-instantclient12.2-basic-12.2.0.1.0-1.x86_64.rpm 
rpm -ivh oracle-instantclient12.2-devel-12.2.0.1.0-1.x86_64.rpm
更新全局变量，首先vi进入文件

vi /etc/profile
文件末尾加入

export ORACLE_HOME=/usr/lib/oracle/12.2/client64
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
export NLS_LANG="AMERICAN_AMERICA.ZHS16GBK"
立即生效环境变量
source /etc/profile

C_INCLUDE_PATH=/usr/include/oracle/12.2/client64 pecl install oci8

一路回车
最后看见
You should add "extension=oci8.so" to php.ini

--------------------- 
作者：都市放猪 
来源：CSDN 
原文：https://blog.csdn.net/wq57885/article/details/80871740 
版权声明：本文为博主原创文章，转载请附上博文链接！

```





```javascript
vim /etc/php.d/oci8.ini
extension=oci8.so

service httpd restart

phpinfo中显示有oci8相关内容

```



安装pdo_oci8:未完成

```javascript
tp5显示依然显示错误could not find driver，为什么？

因为它用的pdo，还少一个：pdo_oci.so

去 https://pecl.php.net/package/PDO_OCI 下载PDO_OCI-1.0.tgz

可以wget，我这里是通过win7下载上传到虚拟机里

我的下载地址https://download.csdn.net/download/wq57885/10511343

解压包

tar -xvf PDO_OCI-1.0.tgz
首先需要在包的config.m4里加两行代码

Find a pattern like this near line 10 and add these 2 lines:

elif test -f $PDO_OCI_DIR/lib/libclntsh.$SHLIB_SUFFIX_NAME.12.1; then
PDO_OCI_VERSION=12.2
Find a pattern like this near line 101 and add these lines:

12.2)
  PHP_ADD_LIBRARY(clntsh, 1, PDO_OCI_SHARED_LIBADD)
  ;;
在pdo_oci.c文件中

    将 function_entry 改成 zend_function_entry

提前解决后面编译会出现的（这步花了若干时间，google到的方法）

/usr/bin/ld: cannot find -lclntsh

collect2: error: ld returned 1 exit status

cd /usr/lib
ln -s $ORACLE_HOME/lib/libclntsh.so libclntsh.so
--------------------- 
作者：都市放猪 
来源：CSDN 
原文：https://blog.csdn.net/wq57885/article/details/80871740 
版权声明：本文为博主原创文章，转载请附上博文链接！

```



```javascript
ln -s  /usr/lib/oracle/12.2/client64  /usr/lib/oracle/12.2/client
ln -s /usr/include/oracle/12.2/client64 /usr/include/oracle/12.2/client

```

https://www.cnblogs.com/jpdoutop/p/Install-PDO_OCI-extension-On-Linux-64bit.html





```javascript
进入PDO_OCI-1.0目录下，我这里省略命令

 /usr/bin/phpize
 ./configure --with-pdo-oci=instantclient,/usr,12.2  
 make && make install  
 echo "extension=pdo_oci.so" > /etc/php.d/pdo_oci.ini  
 service php-fpm restart
--------------------- 
作者：都市放猪 
来源：CSDN 
原文：https://blog.csdn.net/wq57885/article/details/80871740 
版权声明：本文为博主原创文章，转载请附上博文链接！

```



