---
tags:
    - 操作系统
    - Mac
---

Mac OSX 下XAMPP 自动启动

http://jonathannicol.com/blog/2012/03/12/launching-xampp-at-osx-startup/



Launching XAMPP at OSX startup

A few days ago I posted about my experiences setting up XAMPP on OSX. Here’s another little XAMPP tip…

By default XAMPP won’t start the Apache and MySQL services at system startup, so every time you reboot your computer you’ll need to restart them. Wouldn’t it be nice if those services started automatically? One way of doing that is to create a Launch Daemon that runs at system startup and have it start XAMPP for us.

Fire up Terminal, and run the following command:

cd /Library/LaunchDaemons
sudo nano apachefriends.xampp.apache.start.plist

Enter your OSX password when prompted, then in nano paste the following into your new plist:

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
<key>EnableTransactions</key>
<true/>
<key>Label</key>
<string>apachefriends.xampp.apache.start</string>
<key>ProgramArguments</key>
<array>
<string>/Applications/XAMPP/xamppfiles/xampp</string>
<string>startapache</string>
</array>
<key>RunAtLoad</key>
<true/>
<key>WorkingDirectory</key>
<string>/Applications/XAMPP/xamppfiles</string>
<key>KeepAlive</key>
<false/>
<key>AbandonProcessGroup</key>
<true/>
</dict>
</plist>

Save the file and exit nano (control+o, return, control+x).

Now run the following terminal command:

sudo nano apachefriends.xampp.mysql.start.plist

And into this new file paste:

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
<key>EnableTransactions</key>
<true/>
<key>Label</key>
<string>apachefriends.xampp.mysql.start</string>
<key>ProgramArguments</key>
<array>
<string>/Applications/XAMPP/xamppfiles/xampp</string>
<string>startmysql</string>
</array>
<key>RunAtLoad</key>
<true/>
<key>WorkingDirectory</key>
<string>/Applications/XAMPP/xamppfiles</string>
<key>KeepAlive</key>
<false/>
<key>AbandonProcessGroup</key>
<true/>
</dict>
</plist>

Save the file and exit nano (control+o, return, control+x).

When you restart your computer the XAMPP Apache and MySQL services should start automatically. You can check this by launching XAMPP Control and checking that Apache and MySQL have green lights displayed next to them.

A note about security

If you’re concerned about the security of your system while running XAMPP, the safest approach is not to run Apache or MySQL at all, in which case you might not want to have those services running while you’re not using them. However, I’m fairly certain that unless you intentionally open up port 80 in your hardware/software firewall your XAMPP server should be invisible outside your local network.

Credit

A hat tip to ‘cwd’, who posted this solution on Superuser. I tried a couple of other approaches before I stumbled upon this one which actually works.

