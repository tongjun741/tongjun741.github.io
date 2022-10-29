---
tags:
    - 其它
    - 深度学习
---

SSH 连接 Google colab

前言

在网上参考的利用 SSHD 和 ngrok 实现远程连接 Google colab，具体参考 Gist. 如果不喜欢 colab 的 web 工作环境，以及要加特殊符号的 shell 命令，可参考本文，亲测。



步骤

1. 进入 google 云盘创建一个 colaboratory jupyter 文件





2. 设置创建的 .ipynb 文件，选择 Python3



点击菜单栏 Edit --> Notebook settings，选择 Python3, GPU 等。当然 Python2 也可以，只是要改下面的代码。





3. 运行脚本，生成 host 和 password



copy 下面代码到 ipynb 的一个 cell 中，ctrl + enter 运行cell，

```javascript
import random, string, urllib.request, json, getpass

#Generate root password
password = ''.join(random.choice(string.ascii_letters + string.digits) for i in range(20))

#Download ngrok
! wget -q -c -nc https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
! unzip -qq -n ngrok-stable-linux-amd64.zip

#Setup sshd
! apt-get install -qq -o=Dpkg::Use-Pty=0 openssh-server pwgen > /dev/null

#Set root password
! echo root:$password | chpasswd
! mkdir -p /var/run/sshd
! echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
! echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config
! echo "LD_LIBRARY_PATH=/usr/lib64-nvidia" >> /root/.bashrc
! echo "export LD_LIBRARY_PATH" >> /root/.bashrc

#Run sshd
get_ipython().system_raw('/usr/sbin/sshd -D &')

#Ask token
print("Copy authtoken from https://dashboard.ngrok.com/auth")
authtoken = getpass.getpass()

#Create tunnel
get_ipython().system_raw('./ngrok authtoken $authtoken && ./ngrok tcp 22 &')

#Get public address and print connect command
with urllib.request.urlopen('http://localhost:4040/api/tunnels') as response:
  data = json.loads(response.read().decode())
  (host, port) = data['tunnels'][0]['public_url'][6:].split(':')
  print(f'SSH command: ssh -p{port} root@{host}')

#Print root password
print(f'Root password: {password}')

```

运行过程中会提示输入 ngrok 生成的 authtoken，如下图，





点击生成的连接进入，copy authtoken，粘贴进提示框，如果没有注册 ngrok 的需先注册，cell 继续运行，最终的输出类似于：



Copy authtoken from https://dashboard.ngrok.com/auth

··········

SSH command: ssh -p19483 root@0.tcp.ngrok.io

Root password: G9azUpC7XmDkNyXg***



4. 在个人 pc 端 ssh 连接



输入下面的命令，对应的 port 和 password 根据上面的生成的为例，‘19483’ 为端口号



ssh -p19483 root@0.tcp.ngrok.io





可以看到 colab 提供的免费显卡，



nvidia-smi





此外想挂载 google 云盘到 colab，则上面的 ipynb 文件中添加一个 cell，运行下面代码，并点击生成的链接认证，并 copy 生成的 authorization code 完成挂载。



from google.colab import drive

drive.mount('/content/drive/')



则可以在远程连接的 colab 中的 /content/drive/ 路径中看到网路云盘的内容。

--------------------- 

作者：MT2048 

来源：CSDN 

原文：https://blog.csdn.net/u010986080/article/details/93505201 

版权声明：本文为博主原创文章，转载请附上博文链接！

