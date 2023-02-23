---
tags:
    - SS
---

```
yum install wget curl telnet net-tools zip unzip ntpdate vim  git python3 -y

export http_proxy=http://192.168.5.233:8118
export https_proxy=http://192.168.5.233:8118
wget https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh
chmod +x install-release.sh
./install-release.sh -p http://192.168.5.233:8118
  
systemctl enable v2ray
systemctl start v2ray
systemctl disable firewalld
systemctl stop firewalld

mkdir -p /etc/v2ray
ln -s /usr/local/etc/v2ray/config.json  /etc/v2ray/config.json

# 修改 /etc/v2ray/config.json中的监听的ip与端口
# 一定要确定时间没有问题

git clone https://github.com/xbblog95/v2sub.git
cd v2sub
chmod +x v2sub

# 可以修改shadowsocks.py  v2ray.py  中监听IP与端口（见下面的配置）
# 可以修改v2sub的配置文件路径为 /usr/local/etc/v2ray/config.json
pip3 install requests  -i https://pypi.douban.com/simple/
# 更换节点
./v2sub

# 测试
curl -x socks5://127.0.0.1:1080 http://www.google.com -i
curl -x socks5://192.168.5.108:1080 http://www.google.com -i
curl -x http://192.168.5.108:8118 http://www.google.com -i
```



```
"inbounds": [
    {
      "listen": "0.0.0.0",
      "protocol": "socks",
      "settings": {
        "auth": "noauth"
      },
      "port": "1080"
    },
    {
      "listen": "0.0.0.0",
      "protocol": "http",
      "settings": {
        "timeout": 360
      },
      "port": "8118"
    }
  ],
```

