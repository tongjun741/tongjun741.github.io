---
tags:
    - 其它
    - Git
---

## 手动触发按钮

在时隔多年后 GitHub Ac­tions 终于引入了一个手动触发的按钮，不过默认是不开启的，需要在 work­flow 文件中设置 `workflow_dispatch` 触发事件。一个最简单的例子：

```none
on:
  workflow_dispatch:
```

设置好触发事件后就能在相关 work­flow 的页面下看到 `Run workflow` 按钮.



[![img](/img-post/开发/其它/Git/GitHub Actions 手动触发方式.assets/20200817194613.png)](https://img.p3terx.com/post/20200817194613.png)



更复杂一点还可以实现在手动触发时填写参数，控制不同的工作流程或者直接改写某个环境变量等操作。目前官方文档已经相当完善，所以博主就不赘述了，感兴趣的小伙伴可以去参考[官方文档中的相关章节](https://p3terx.com/go/aHR0cHM6Ly9kb2NzLmdpdGh1Yi5jb20vY24vYWN0aW9ucy9jb25maWd1cmluZy1hbmQtbWFuYWdpbmctd29ya2Zsb3dzL2NvbmZpZ3VyaW5nLWEtd29ya2Zsb3cjbWFudWFsbHktcnVubmluZy1hLXdvcmtmbG93)。



https://p3terx.com/archives/github-actions-manual-trigger.html