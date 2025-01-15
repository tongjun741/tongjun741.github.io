---
tags:
    - 其它
    - Git
    - GitHub Actions
---

默认情况下GitHub Actions的macOS环境中屏幕录像和控制是禁止的，启动远程桌面会提示如下错误：

```
Screen recording might be disabled. Screen Sharing or Remote Management must be enabled from System Settings or via MDM.
Screen control might be disabled. Screen Sharing or Remote Management must be enabled from System Settings or via MDM.
```



需要通过特别的脚本来hack：

```
#!/bin/bash

# A cleaner alternative to this approach, but which requires a restart, is to populate TCC's SiteOverrides.plist inside
# the TCC app support directory with the following:
# <?xml version="1.0" encoding="UTF-8"?>
# <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
# <plist version="1.0">
# <dict>
# 	<key>Services</key>
# 		<dict>
# 		<key>PostEvent</key>
# 		<array>
# 			<dict>
# 				<key>Allowed</key>
# 				<true/>
# 				<key>CodeRequirement</key>
# 				<string>identifier "com.apple.screensharing.agent" and anchor apple</string>
# 				<key>Identifier</key>
# 				<string>com.apple.screensharing.agent</string>
# 				<key>IdentifierType</key>
# 				<string>bundleID</string>
# 			</dict>
# 		</array>
# 		<key>ScreenCapture</key>
# 		<array>
# 			<dict>
# 				<key>Allowed</key>
# 				<true/>
# 				<key>CodeRequirement</key>
# 				<string>identifier "com.apple.screensharing.agent" and anchor apple</string>
# 				<key>Identifier</key>
# 				<string>com.apple.screensharing.agent</string>
# 				<key>IdentifierType</key>
# 				<string>bundleID</string>
# 			</dict>
# 		</array>
# 	</dict>
# </dict>
# </plist>

set -eux -o pipefail

db_path="/Library/Application Support/com.apple.TCC/TCC.db"

sanity_checks() {
  os_ver_major="$(sw_vers -productVersion | awk -F'.' '{print $1}')"
  if [[ "${os_ver_major}" -ne 12 ]]; then
    echo "This script is only tested valid on macOS 12, and we detected this system runs version ${os_ver_major}. Exiting."
    exit 1
  fi

  if [[ "$(id -u)" -ne 0 ]]; then
    echo "Need to run this script as root... exiting"
    exit 1
  fi

  # TODO: we should bail if we determine we don't have write access to the TCC db (we want to get specific SIP disable status
  #       for whatever is the protection for TCC)
}

disable_screensharing() {
  launchctl unload -w /System/Library/LaunchDaemons/com.apple.screensharing.plist
  sqlite3 "${db_path}" \
    "BEGIN TRANSACTION; \
     DELETE FROM access WHERE client = 'com.apple.screensharing.agent'; \
     COMMIT;"
}

enable_screensharing() {
  launchctl load -w /System/Library/LaunchDaemons/com.apple.screensharing.plist

  epoch="$(date +%s)"
  sqlite3 "${db_path}" \
    "BEGIN TRANSACTION; \
     DELETE FROM access WHERE client = 'com.apple.screensharing.agent'; \
     COMMIT; \

     BEGIN TRANSACTION; \
     INSERT INTO access VALUES('kTCCServicePostEvent','com.apple.screensharing.agent',0,2,4,1,NULL,NULL,0,'UNUSED',NULL,0,${epoch}); \
     INSERT INTO access VALUES('kTCCServiceScreenCapture','com.apple.screensharing.agent',0,2,4,1,NULL,NULL,0,'UNUSED',NULL,0,${epoch}); \
     COMMIT;"
}


dump_screensharing_entries() {
sqlite3 "${db_path}" \
  "SELECT * FROM access WHERE client = 'com.apple.screensharing.agent';"
}

# uncomment to show existing entries for debugging
# dump_screensharing_entries

sanity_checks

enable_screensharing
# uncomment to disable instead of enable
# disable_screensharing


```

来源：

https://gist.githubusercontent.com/timsutton/31344ef60dbd4d64aca5b3287c0644e8/raw/e1fb343f3fc9ac30cea5f4ee14ba7a1384499d5e/modify_screensharing.sh