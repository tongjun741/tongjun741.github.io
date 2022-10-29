---
tags:
    - IDE
---

IntelliJ IDEA 配置文件存放的位置

IDE settings are stored in the dedicated directories under the product home directory, depending on the platform. The product home directory name is composed of the product name and version.

For IntelliJ IDEA Community edition the folder name is .IdeaICXX.

For example:

Windows

- <User home>\.IntelliJIdeaXX\config that contains user-specific settings.

- <User home>\.IntelliJIdeaXX\system that stores IntelliJ IDEA data caches.

<User home> in WindowsXP is C:\Documents and Settings\<User name>\; in Windows Vista it is C:\Users\<User name>\

Linux

- ~/.IntelliJIdeaXX/config that contains user-specific settings.

- ~/.IntelliJIdeaXX/system that stores IntelliJ IDEA data caches.



Mac OS

- ~/Library/Application Support/IntelliJIdeaXX contains the catalog with plugins.

- ~/Library/Preferences/IntelliJIdeaXX contains the rest of the configuration settings.

- ~/Library/Caches/IntelliJIdeaXX contains data caches, logs, local history, etc. These files can be quite significant in size.

- 9.0+~/Library/Logs/IntelliJIdeaXX contains logs

The config directory has several subfolders that contain xml files with your personal settings. You can easily share your preferred keymaps, color schemes, etc. by copying these files into the corresponding folders on another IntelliJ IDEA installation. Prior to copying, make sure that IntelliJ IDEA is not running, because it can erase the newly transferred files before shutting down.

The following is the list of some of the subfolders under the config folder, and the settings contained therein.





http://normalme.lofter.com/post/2bd70f_25ea374

