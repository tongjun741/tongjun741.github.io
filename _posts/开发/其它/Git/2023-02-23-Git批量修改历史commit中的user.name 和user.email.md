---
tags:
    - 其它
    - Git
---

**1.克隆仓库**

注意参数，这个不是普通的clone，clone下来的仓库并不能参与开发

```
git clone --bare https://github.com/user/repo.git
cd repo.git
```

**2.命令行中运行代码**

OLD_EMAIL原来的邮箱 
CORRECT_NAME更正的名字 
CORRECT_EMAIL更正的邮箱

将下面代码复制放到命令行中执行

```
git filter-branch -f --env-filter '
OLD_EMAIL="wowohoo@qq.com"
CORRECT_NAME="小弟调调"
CORRECT_EMAIL="更正的邮箱@qq.com"
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

执行过程

```
Rewrite 160d4df2689ff6df3820563bfd13b5f1fb9ba832 (479/508) (16 seconds passed, remaining 0 predicted)
Ref 'refs/heads/dev' was rewritten
Ref 'refs/heads/master' was rewritten
```

**3.同步到远程仓库**

同步到push远程git仓库

```
git push --force --tags origin 'refs/heads/*'
```

我还遇到了如下面错误，lab默认给master分支加了保护，不允许强制覆盖。`Project(项目)`->`Setting`->`Repository` 菜单下面的`Protected branches`把master的保护去掉就可以了。修改完之后，建议把master的保护再加回来，毕竟强推不是件好事。

```
remote: GitLab: You are not allowed to force push code to a protected branch on this project.
```

当上面的push 不上去的时候需要在gitlab上将该分支的保护去掉



https://blog.csdn.net/while0/article/details/89178756

https://www.jianshu.com/p/662c5816ac19

