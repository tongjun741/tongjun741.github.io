---
tags:
    - 操作系统
    - Mac
    - 软件安装
---

在区分大小写的分区安装Photoshop CS6

https://github.com/tzvetkoff/adobe_case_sensitive_volumes



http://tzvetkoff.net/posts/2013/05/20/adobe-cs6-on-case-sensitive-drives.html



Installing Adobe CS6 on case-sensitive drives (Mac OS X)

Well, everybody knows that Adobe are a [censored] company. Their products are the defacto standard for image/video editing and designing, but their codebase really suck. No excuses.

The problem addressed here is that Creative Studio™ refuses to install on a case-sensitive drive on Mac OS X. And it doesn't just refuse to install on a case-sensitive drive, but it also requires to install on your boot drive as well! Srsly?

Well, there's a solution. I've just stumbled upon this, and I'm really anxious to share it. I've forked the code to update it for CS6.

Prerequisites

1. Xcode

You can install it from AppStore.

1. Command Line Tools for Xcode.

You can install it from Xcode's Preferences -> Downloads.

A step-by-step installation instructions

1. Create a .sparsebundle pseudo-image to install CS6:

```javascript
mkdir -p ~/Stuff/Adobe
cd ~/Stuff/Adobe
hdiutil create -size 15g -type SPARSEBUNDLE -nospotlight -volname 'Adobe CS6 SparseBundle' -fs 'Journaled HFS+' ~/Stuff/Adobe/Adobe_CS6_SparseBundle.sparsebundle

```

1. Mount the newly created image and create a /Adobe directory inside

```javascript
open ~/Stuff/Adobe/Adobe_CS6_SparseBundle.sparsebundle
mkdir -p /Volumes/Adobe\ CS6\ SparseBundle/Adobe

```

1. Create an extra /Applications/Adobe folder on the boot drive (we will trick the installer with this temporary directory.)

```javascript
mkdir -p /Applications/Adobe

```

1. Get the hack, compile it, and run it

OK, at this point you'll need to edit the Makefile and set the CS6_INSTALLER_PATH variable to point to the Install.app directory. The current one tries to find it automatically, but it may fail...

```javascript
cd ~/Stuff/Adobe
git clone git://github.com/tzvetkoff/adobe_case_sensitive_volumes.git
cd adobe_case_sensitive_volumes
make
sudo make run

```

1. When asked, select /Applications/Adobe for installation directory rather than just /Applications, but don't click the Install button!! Remember, don't click the Install button just yet.

1. Now, time to do one more hack - remove the /Applications/Adobe directory and replace it with a symlink to the /Adobe directory from the SparseBundle.

```javascript
rm -rf /Applications/Adobe
ln -s /Volumes/Adobe\ CS6\ SparseBundle/Adobe/ /Applications/Adobe

```

1. Now click the Install button

1. You can now safely delete the intermediate files and probably move the SparseBundle somewhere easier to mount by just clicking it (the Desktop, probably?)

```javascript
mv ~/Stuff/Adobe/Adobe_CS6_SparseBundle.sparsebundle ~/Desktop/Adobe_CS6_SparseBundle.sparsebundle
rm -rf ~/Stuff/Adobe

```

1. That's it!

Just remember that you'll need to mount the SparseBundle every time you need to use Adobe's products.

Thanks

lokkju, for writing that awesome article and code to start from

