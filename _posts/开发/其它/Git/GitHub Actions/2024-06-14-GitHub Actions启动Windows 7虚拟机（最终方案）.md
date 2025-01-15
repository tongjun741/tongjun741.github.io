---
tags:
    - 其它
    - Git
    - GitHub Actions
---

启发：https://github.com/dockur/windows

这个项目是在开启KVM的Linux上运行docker容器，容器中启动Windows。



相关参考：
https://actuated.dev/blog/kvm-in-github-actions
https://jnsgr.uk/2024/02/nixos-vms-in-github-actions/
https://github.blog/changelog/2023-02-23-hardware-accelerated-android-virtualization-on-actions-windows-and-linux-larger-hosted-runners/
https://ostechnix.com/how-to-use-vagrant-with-libvirt-kvm-provider/

https://joachim8675309.medium.com/devops-box-vagrant-with-kvm-d7344e79322c
https://github.com/adrahon/vagrant-kvm
https://netlab.tools/labs/libvirt/
https://ostechnix.com/how-to-use-vagrant-with-libvirt-kvm-provider/
https://app.vagrantup.com/stooj/boxes/windows7



    sudo adduser $USER kvm
    sudo chown -R $USER /dev/kvm





NFS

```
sudo apt-get install nfs-common nfs-kernel-server
```

