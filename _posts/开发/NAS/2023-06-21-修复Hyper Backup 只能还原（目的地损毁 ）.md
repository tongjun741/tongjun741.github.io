---
tags:
    - NAS
---

I had a similar issue for a [DS418j](https://www.synoforum.com/resources/diskstation-ds418j.37/) (source) and DS416 (destination). You could try the following steps. YMMV.

"Src" is the name for the source NAS and "Dst" is the name for the destination NAS.

Before proceeding,
a. Disable any backup schedules.
b. Check that your disks and memory are all performing well. If there is any hardware fault, fix it first.

SSH into **Dst** to perform the following steps.

1. Change to root.

```
sudo -i
```

2. Change into backup directory.
   ```
   cd /volumeUSB1/usbshare/tongjun-nas_1.hbk
   ```

   

3. Check the status. You should get "detect-bad".

   ```
   sqlite3 Config/target_info.db
   执行：
   select status from target_info;
   可以看到输出：detect-bad
   退出：
   .exit
   ```

4. Change to HyperBackup Vault's bin folder and run synoimgbkptool.
   ```
   cd /var/packages/HyperBackup/target/bin/
   nohup ./synoimgbkptool -r /volumeUSB1/usbshare/tongjun-nas_1.hbk -t Dst -R detect > /volume2/\@tmp/recover.output &
   ```

   

You may get "nohup: ignoring input and redirecting stderr to stdout". It's ok to ignore this.

5. Wait for synoimgbkptool to complete running. It can take a very long time, 3 to 4 days. You can check with the following command.

```
ps aux | grep synoimgbkptool
```

If you see something similar to the following, it is still running.


Otherwise, you should see something similar to this.

You can now exit the SSH to Dst.

Next, SSH into **Src**.

1. Change to root.

```
sudo -i
```

2. Change to HyperBackup's config directory.
   ```
   cd /var/synobackup/config/
   ```

   

3. Edit task_state.conf (using vi) to the following.
   ```
   last_state="Backupable"
   state="Backupable"
   ```

   

You can now exit SSH to Src.

Finally, log in to the admin portal. Start HyperBackup and choose "Check backup integrity". It can take a long time (3 to 4 days). Once that's completed without errors, you should be able to resume your backup schedule.

Hope that helps.



https://www.synoforum.com/threads/destination-corrupted-error-in-hyperbackup.865/page-2