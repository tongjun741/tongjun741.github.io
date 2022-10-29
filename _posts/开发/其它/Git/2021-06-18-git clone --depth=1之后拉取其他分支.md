---
tags:
    - 其它
    - Git
---

git clone --depth=1之后拉取其他分支



```javascript
当项目过大时，git clone会出现超时失败，这时候我们可以只拉去最新的一次或者几次commit:

git clone --depth=1 xxxxxx
    
有时候，我们总是图一时之快，而忽略了其他的问题。
你可能很快会发现，拉下来的只是默认的分支，而要想拉取其他分支代码的时候，一脸懵逼，有木有？？
其实也很简单：

git remote set-branches origin 'remote_branch_name'
git fetch --depth 1 origin remote_branch_name
git checkout remote_branch_name

搞定收工。

```



https://robberjj.github.io/2018/09/25/GitDepthOtherBr/

