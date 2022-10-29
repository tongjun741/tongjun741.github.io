---
tags:
    - 操作系统
    - Mac
---

Mac下NTFS分区dirty的修复方法

sudo vi /etc/fstab，然后把Elements改为移动硬盘的名字再重新挂载就可以了

LABEL=Elements none ntfs rw,auto,nobrowse
LABEL=AB\040DE none ntfs rw,auto,nobrowse

- \040代表空格

- 注意：不一定每个移动硬盘都好使

当无法使用上面的方法正确加载，并且sudo dmesg发现类似NTFS volume is dirty. You should unmount it and run chkdsk这样的信息时，可以使用ntfs-3g这个包来修复一下NTFS分区

通常这种情况发生在移动硬盘在Windows机器上没有正确拔出之后

安装ntfs-3g：

# 确保brew是最新版本
brew update

# 安装ntfs-3g包依赖的osxfuse
brew cask install osxfuse

# 安装ntfs-3g
brew install homebrew/fuse/ntfs-3g

重新插入移动硬盘，如果系统自动挂载则在Finder推出，然后执行修复动作：

# 得到移动硬盘的设备文件地址
diskutil list



HomeBrew安装ntfs-3g后，默认可执行文件在/usr/local/Cellar/ntfs-3g/2017.3.23/bin/ntfsfix

# 修复NTFS分区
sudo ntfsfix /dev/disk2s1

# 如果上面那条不行，就再来这个猛的
sudo ntfsfix -d /dev/disk2s1

通常执行完之后再重新mount就能正常使用rw模式挂载了：

sudo mount -t ntfs -o rw,auto,nobrowse /dev/disk2s1 /Volumes/a

如果嫌自己敲这些命令麻烦，就使用商业软件Paragon NTFS吧



https://segmentfault.com/a/1190000005850790





How to fix corrupted NTFS drive on Mac OS X

![](http://4.bp.blogspot.com/-quD5HlOy6kY/U09OjnYavNI/AAAAAAAAI1c/7l9b_SYGk8Q/s1600/paragon+ntfs+icon.png)

OSX can only read from NTFS disks, although there are hacks to make NTFS disks writable, and there NTFS Free and FUSE for OS X. The most popular option to use NTFS disks on Mac OS X is using Paragon's NTFS for Mac OS X.

are options such as 





Windows' chkdsk

I started facing crashes on OS X in the past two weeks. From my observations, the only common factor was writing to the external USB hard disk that is in NTFS format. Specifically downloading from a web browser and saving directly into the external drive. In addition, ntfsfix has also indicated some errors; we will come back to ntfsfix later.



As Microsoft is the company that created and define the NTFS filesystem, the safest way to fix any NTFS errors is to use Microsoft's tool. Run Disk error checking from Windows Explorer, or chkdsk /r in the command prompt.



![](http://3.bp.blogspot.com/-UcTBxDW0M2c/U09H_X9z8II/AAAAAAAAI1E/oWSFFumywmk/s1600/windows-7-disk-error-check1.jpg)

Source: http://www.dailyartisan.com/news/windows-7-tips/



Paragon NTFS

I have been using Paragon NTFS for almost a year and it works well. The benefit of Paragon NTFS is having a well-integrated plug-in in System Preferences, as well as integration with other OS X disk utilities. In my case, I can use Disk Utility to repair the NTFS disk.



Click on the NTFS disk on the left pane. Note that you will have to select the Volume name, not just the device name. In the main area, click on First Aid and Repair Disk. You should see the progress and details of disk repair.



![](http://2.bp.blogspot.com/-hgJ6y9i6PQo/U09K8HLtIzI/AAAAAAAAI1Q/rkE0K2ZqzxA/s1600/Disk_Utility_NTFS_Repair.png)





fsck_ufsd_NTFS



No, it is not a vulgar term in jargon, but an equivalent command line utility provided by Paragon: /sbin/fsck_ufsd_NTFS. You will first need to get the disk device name for the NTFS volume.



$ diskutil list
/dev/disk0
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *240.1 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                  Apple_HFS Sandisk 240G            239.2 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
/dev/disk1
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *1.0 TB     disk1
   1:               Windows_NTFS Seagate Expansion Drive 1.0 TB     disk1s1







The disk device to be used is /dev/disk1s1. You need superuser privileges to run fsck_ufsd_NTFS



$ sudo fsck_ufsd_NTFS -n /dev/disk1s1 
Checking Volume /dev/disk1s1...                                                
Type of the filesystem is NTFS.                                                
Volume label is: Seagate Expansion Drive.                                      
$UpCase file is formatted for use in Windows 7.                                
    604.67 GB in 424070 files.                                                 
    209868 KB in 104771 directories.                                           
         0 KB in bad blocks in 0 fragments.                                    
    836352 KB in use by the system.                                            
     65536 KB occupied by the log file.                                        
      4096 bytes in each allocation unit.                                      
 244190000 total allocation units on volume.                                   
  63759638 allocation units available on volume.                               









No repairs were necessary for volume /dev/disk1s1.  





NTFS-3G



While Paragon NTFS is powerful and easy to use, it is a commercial software that you need to pay for. A free alternative is to use NTFS-3G. NTFS-3G ships with an utility called ntfsfix that can repair NTFS disk errors. 



Note that you will need to unmount the volume before running ntfsfix, or you will see this error message. 





$ sudo ntfsfix /dev/disk1s1 
Mounting volume... Error opening '/dev/disk1s1': Resource busy
FAILED
Attempting to correct errors... Error opening '/dev/disk1s1': Resource busy
FAILED
Failed to startup volume: Resource busy
Error opening '/dev/disk1s1': Resource busy


Volume is corrupt. You should run chkdsk.









Unmount the volume and run ntfsfix with superuser privileges.



$ sudo umount /Volumes/Seagate\ Expansion\ Drive/
$ sudo ntfsfix /dev/disk1s1 
Mounting volume... OK
Processing of $MFT and $MFTMirr completed successfully.
Checking the alternate boot sector... OK
NTFS volume version is 3.1.

NTFS partition /dev/disk1s1 was processed successfully.



http://hanxue-it.blogspot.com/2014/04/how-to-fix-corrupted-ntfs-drive-on-mac.html



