---
tags:
    - 其它
    - Git
---

gitee导出blog
https://gitee.com/help/articles/4114

https://blog.csdn.net/qq_36667170/article/details/79318578



```
if [[ -z $(git status -s) ]] then

  echo "开始执行命令"

  git add .

  git status

  #写个sleep 1s 是为了解决并发导致卡壳

  sleep 1s

  echo "####### 添加文件 #######"

  git commit -m "save"

  echo "####### commit #######"

  sleep 1s

  echo "####### 开始推送 #######"

  if [ ! $3 ]
  then
    echo "####### 请输入自己提交代码的分支 #######"
    exit;
  fi

	git pull
  git push 

fi
```



https://blog.csdn.net/10km/article/details/100689481

https://blog.csdn.net/weixin_44991965/article/details/106145125