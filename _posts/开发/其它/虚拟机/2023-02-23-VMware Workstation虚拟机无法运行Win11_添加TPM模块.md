---
tags:
    - 其它
    - 虚拟机
---

在用VMware虚拟机安装Windows11时，首先是使用免费的VMware Workstation Player，但很快发现它的可定制能力和功能太弱，无法开启安装Windows11必须TPM2.0可信平台模块。于是酋长转而使用VMware Workstation Pro，没想到刚一开始安装，仍然遇到了“这台电脑无法运行Windows11”的错误提示：





> 这台电脑不符合安装此版本的Windows所需的最低系统要求。有关详细信息，请访问



![img](/img-post/开发/其它/虚拟机/VMware Workstation虚拟机无法运行Win11_添加TPM模块.assets/b3b7d0a20cf431adfd89235f02b532a62fdd988d.jpeg)



酋长立即意识到这肯定还是 TPM 模块的问题，于是经过一番折腾，终于理清了VMware Workstation Pro虚拟机添加 TPM2.0可信平台模块顺利安装Windows11的方法步骤，下面与大家分享一下：





### 一、固件类型选择“UEFI安全引导”





在新建虚拟机的过程中进行到“固件类型”这一步时，请勾选“ UEFI安全引导 ”。如图：



![img](/img-post/开发/其它/虚拟机/VMware Workstation虚拟机无法运行Win11_添加TPM模块.assets/3b292df5e0fe9925a982c1497d2bc0d68cb171ad.jpeg)



不过，即使这一步没有勾选，创建好虚拟机后，后期依然可以编辑虚拟机设置，方法是：





在“虚拟机设置”窗口切换到“选项”，选中“高级”，在右侧的“固件类型”区域就可以选择 “UEFI安全引导” 了。如图：



![img](/img-post/开发/其它/虚拟机/VMware Workstation虚拟机无法运行Win11_添加TPM模块.assets/3bf33a87e950352aad2c0d866ac065fbb3118b7f.jpeg)



### 二、为虚拟机设置密码





你需要为虚拟机设置密码，否则你在 “虚拟机设置” 中添加TPM可信平台模块时，会发现“完成”按钮为灰色，无法完成添加，提示你“虚拟机必须以加密并使用UEFI固件”。如图：



![img](/img-post/开发/其它/虚拟机/VMware Workstation虚拟机无法运行Win11_添加TPM模块.assets/b90e7bec54e736d1b27bb5f6a1d3d1cbd4626974.jpeg)



虚拟机加密的方法如下：





“虚拟机设置”窗口切换到“选项” ，选中“访问控制”，然后在右侧点击“加密”设置密码即可。如图：



![img](/img-post/开发/其它/虚拟机/VMware Workstation虚拟机无法运行Win11_添加TPM模块.assets/8ad4b31c8701a18be73cc799a8ac99012a38fe9a.jpeg)



### 三、添加TPM2.0可信平台模块





在加密虚拟机后，现在就可以顺利添加TPM可信平台模块了，方法是：





在 “虚拟机设置”窗口 的“硬件”选项卡点击底部的“添加”按钮，在弹出的“添加硬件向导”中选中底部的“可信平台模块”，就可以点击“完成”顺利添加了。如图：



![img](/img-post/开发/其它/虚拟机/VMware Workstation虚拟机无法运行Win11_添加TPM模块.assets/6609c93d70cf3bc7e2bc9704e78324a8cc112a22.jpeg)



添加后会显示在设备列表的最底部。如图：



![img](/img-post/开发/其它/虚拟机/VMware Workstation虚拟机无法运行Win11_添加TPM模块.assets/5bafa40f4bfbfbedb87f0afb4173693faec31f9f.jpeg)



OK，现在你就可以在 VMware Workstation Pro虚拟机中顺利安装Windows11了。



https://baijiahao.baidu.com/s?id=1719925104567698677&wfr=spider&for=pc