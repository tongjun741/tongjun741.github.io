---
tags:
    - 操作系统
    - Linux
---

ubuntu18 命令行下开启rdp server 



```javascript
sudo apt-get update
sudo apt-get install -y ubuntu-gnome-desktop xrdp
sudo sed -i 's/allowed_users=console/allowed_users=anybody/' /etc/X11/Xwrapper.config
sudo /etc/init.d/xrdp start

```

Adding a Network Security Group rule for RDP traffic

Connecting via RDP



```javascript
cd /etc/NetworkManager
save NetworkManager.conf to NetworkManager.orig (as a backup)
sudo vi NetworkManager.conf
Change managed=false to managed=true
New file looks like this:
[main]
    plugins=ifupdown,keyfile
[ifupdown]
    managed=true
[device]
    wifi.scan-rand-mac-address=no

sudo service network-manager restart
cd /etc/netplan
sudo vi 50-cloud-init.yaml
Add this line just below network:
renderer: NetworkManager
New file looks similar to this (ensure the renderer line is indented as shown):
network:
    renderer: NetworkManager
    ethernets:
        enp3s0:
            addresses: []
            dhcp4: true
version: 2

save
sudo netplan apply
I had then to restart the computer for this to be effective.
After restart the network will now show "wired-connected"
Then you can go to Settings » Sharing » Screen Sharing
You should now be able to toggle Screen Sharing to ON
Under Networks (bottom of dialog), toggle those ON as well

```



开启远程桌面





http://www.techkb.onl/azure-installing-gnome-desktop-and-xrdp-to-access-an-ubuntu-1710-server/



https://dzone.com/articles/standing-up-an-ubuntu-linux-vm-in-azure

https://askubuntu.com/questions/1060457/ubuntu-18-04-1-lts-cant-enable-screen-sharing

