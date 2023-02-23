---
tags:
    - 操作系统
    - Linux
---

CentOS 7 pdftk prince

pdftk

```javascript
#export http_proxy=http://192.168.5.233:8118;export https_proxy=http://192.168.5.233:8118;
yum localinstall https://www.linuxglobal.com/static/blog/pdftk-2.02-1.el7.x86_64.rpm  -y

```



prince

```javascript
yum localinstall  https://www.princexml.com/download/prince-13.5-1.centos7.x86_64.rpm  -y

```



字体

```javascript
sudo yum groupinstall "fonts" -y

```





PyPDF2

```javascript
yum install python3-pip.noarch -y
pip3  install PyPDF2
python3 a.py


yum install -y epel-release
yum install -y python-pip
pip  install PyPDF2

```



