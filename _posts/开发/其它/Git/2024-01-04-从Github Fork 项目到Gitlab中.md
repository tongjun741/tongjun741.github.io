---
tags:
    - 其它
    - Git
---

## 背景

工作原因，需要将github一个开源项目迁移至内网gitlab，进行二次开发并能更新到github最新代码变动。

## Github ➡️ GitLab（首次）

1. 找一台有外网github访问权限的机器
2. 在这台机器安装git客户端



```undefined
yum install git
```

1. 将github的项目clone下来



```php
git clone https://github.com/xxx/xxxx
```

1. 进入项目根目录，执行一下命令



```undefined
git remote rename origin upstream
```

1. 在内网gitlab上新建同名项目
2. 在第四步的项目根目录下执行



```css
git remote add origin git@<gitlab-server>/<path-to-your-repo>.git
git push -u origin --all
git push -u origin --tags
```

## Github ➡️ GitLab（当GitHub代码有变动时）



```undefined
git pull upstream master
git push origin main
```



作者：zhanglong
链接：https://www.jianshu.com/p/974b52706b5d
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。