---
tags:
    - 其它
    - Git
---

现象：

```
$ git clone git@github.com:xxx/bbb.git
Cloning into 'bbb'...
CreateProcessW failed error:193
ssh_askpass: posix_spawnp: Unknown error
Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```



解决：

```
# 在命令行中执行以下命令，设置全局的sshCommand
git config --global core.sshCommand "'C:\\Program Files\\Git\\usr\\bin\\ssh.exe'"
```

