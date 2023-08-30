---
tags:
    - 其它
    - Git
---

### GitHub Action

https://github.com/marketplace/actions/gitee-pages-action

# Gitee Pages Action

v1.2.8Latest version

Use latest version



# Gitee Pages Action

由于 Gitee Pages 的访问速度很快，很多朋友会选择 Gitee Pages 部署项目（如：个人博客、开源项目国内镜像站点）。但是它不像 GitHub Pages 那样，一提交代码就能自动更新 Pages，因为 Gitee 的自动部署属于 Gitee Pages Pro 的服务。

为了实现 Gitee Pages 的自动部署，我开发了 [Gitee Pages Action](https://github.com/marketplace/actions/gitee-pages-action) ，只需要在 GitHub 项目的 Settings 页面下配置 keys，然后在 `.github/workflows/` 下创建一个工作流，引入一些配置参数即可。欢迎体验，若有使用上的问题，也欢迎随时在 [Issues](https://github.com/yanglbme/gitee-pages-action/issues) 反馈。

注：首次需要**手动**登录 Gitee ，点击“启动”进行 Gitee Pages 服务的部署。

## 入参

| 参数             | 描述                         | 是否必传 | 默认值   | 示例                            |
| ---------------- | ---------------------------- | -------- | -------- | ------------------------------- |
| `gitee-username` | Gitee 用户名                 | 是       | -        | `yanglbme`                      |
| `gitee-password` | Gitee 密码                   | 是       | -        | `${{ secrets.GITEE_PASSWORD }}` |
| `gitee-repo`     | Gitee 仓库（严格区分大小写） | 是       | -        | `doocs/advanced-java`           |
| `branch`         | 要部署的分支（分支必须存在） | 否       | `master` | `main`                          |
| `directory`      | 要部署的分支上的目录         | 否       |          | `src`                           |
| `https`          | 是否强制使用 HTTPS           | 否       | `true`   | `false`                         |

## 示例

以下是一个完整示例。

在你的 GitHub 仓库 `.github/workflows/` 文件夹下创建一个 `.yml` 文件，如 `sync.yml`，内容如下：

```
name: Sync

on: page_build

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Sync to Gitee
        uses: wearerequired/git-mirror-action@master
        env:
          # 注意在 Settings->Secrets 配置 GITEE_RSA_PRIVATE_KEY
          SSH_PRIVATE_KEY: ${{ secrets.GITEE_RSA_PRIVATE_KEY }}
        with:
          # 注意替换为你的 GitHub 源仓库地址
          source-repo: git@github.com:doocs/advanced-java.git
          # 注意替换为你的 Gitee 目标仓库地址
          destination-repo: git@gitee.com:Doocs/advanced-java.git

      - name: Build Gitee Pages
        uses: yanglbme/gitee-pages-action@main
        with:
          # 注意替换为你的 Gitee 用户名
          gitee-username: yanglbme
          # 注意在 Settings->Secrets 配置 GITEE_PASSWORD
          gitee-password: ${{ secrets.GITEE_PASSWORD }}
          # 注意替换为你的 Gitee 仓库，仓库名严格区分大小写，请准确填写，否则会出错
          gitee-repo: doocs/advanced-java
          # 要部署的分支，默认是 master，若是其他分支，则需要指定（指定的分支必须存在）
          branch: main
```

先使用 [wearerequired/git-mirror-action](https://github.com/wearerequired/git-mirror-action) 将 GitHub 仓库同步到 Gitee 仓库，再使用 [yanglbme/gitee-pages-action](https://github.com/yanglbme/gitee-pages-action) 实现 Gitee Pages 的自动部署。

**密钥的配置步骤如下（可展开看示例图）**：

<details open="" style="box-sizing: border-box; display: block; margin-top: 0px; margin-bottom: 16px;"><summary style="box-sizing: border-box; display: list-item; cursor: pointer;">1. 在命令行终端或 Git Bash 使用命令<span>&nbsp;</span><code style="box-sizing: border-box; font-family: SFMono-Regular, Consolas, &quot;Liberation Mono&quot;, Menlo, monospace; font-size: 13.6px; padding: 0.2em 0.4em; margin: 0px; background-color: var(--color-markdown-code-bg); border-radius: 6px;">ssh-keygen -t rsa -C "youremail@example.com"</code><span>&nbsp;</span>生成 SSH Key，注意替换为自己的邮箱。生成的<span>&nbsp;</span><code style="box-sizing: border-box; font-family: SFMono-Regular, Consolas, &quot;Liberation Mono&quot;, Menlo, monospace; font-size: 13.6px; padding: 0.2em 0.4em; margin: 0px; background-color: var(--color-markdown-code-bg); border-radius: 6px;">id_rsa</code><span>&nbsp;</span>是私钥，<code style="box-sizing: border-box; font-family: SFMono-Regular, Consolas, &quot;Liberation Mono&quot;, Menlo, monospace; font-size: 13.6px; padding: 0.2em 0.4em; margin: 0px; background-color: var(--color-markdown-code-bg); border-radius: 6px;">id_rsa.pub</code><span>&nbsp;</span>是公钥。(<g-emoji class="g-emoji" alias="warning" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/26a0.png" style="box-sizing: border-box; font-family: &quot;Apple Color Emoji&quot;, &quot;Segoe UI Emoji&quot;, &quot;Segoe UI Symbol&quot;; font-size: 1em; font-weight: 400; line-height: 1; vertical-align: -0.075em; font-style: normal !important;">⚠️</g-emoji>注意此处不要设置密码)</summary><a target="_blank" rel="noopener noreferrer" href="https://github.com/yanglbme/gitee-pages-action/blob/main/images/gen_ssh_key.png" style="box-sizing: border-box; background-color: initial; color: var(--color-text-link); text-decoration: none;"><img src="https://github.com/yanglbme/gitee-pages-action/raw/main/images/gen_ssh_key.png" alt="gen_ssh_key" style="box-sizing: initial; border-style: none; max-width: 100%; background-color: var(--color-bg-primary);"></a></details>

<details style="box-sizing: border-box; display: block; margin-top: 0px; margin-bottom: 16px;"><summary style="box-sizing: border-box; display: list-item; cursor: pointer;">2. 在 GitHub 项目的「Settings -&gt; Secrets」路径下配置好命名为<span>&nbsp;</span><code style="box-sizing: border-box; font-family: SFMono-Regular, Consolas, &quot;Liberation Mono&quot;, Menlo, monospace; font-size: 13.6px; padding: 0.2em 0.4em; margin: 0px; background-color: var(--color-markdown-code-bg); border-radius: 6px;">GITEE_RSA_PRIVATE_KEY</code><span>&nbsp;</span>和<span>&nbsp;</span><code style="box-sizing: border-box; font-family: SFMono-Regular, Consolas, &quot;Liberation Mono&quot;, Menlo, monospace; font-size: 13.6px; padding: 0.2em 0.4em; margin: 0px; background-color: var(--color-markdown-code-bg); border-radius: 6px;">GITEE_PASSWORD</code><span>&nbsp;</span>的两个密钥。其中：<code style="box-sizing: border-box; font-family: SFMono-Regular, Consolas, &quot;Liberation Mono&quot;, Menlo, monospace; font-size: 13.6px; padding: 0.2em 0.4em; margin: 0px; background-color: var(--color-markdown-code-bg); border-radius: 6px;">GITEE_RSA_PRIVATE_KEY</code><span>&nbsp;</span>存放<span>&nbsp;</span><code style="box-sizing: border-box; font-family: SFMono-Regular, Consolas, &quot;Liberation Mono&quot;, Menlo, monospace; font-size: 13.6px; padding: 0.2em 0.4em; margin: 0px; background-color: var(--color-markdown-code-bg); border-radius: 6px;">id_rsa</code><span>&nbsp;</span>私钥；<code style="box-sizing: border-box; font-family: SFMono-Regular, Consolas, &quot;Liberation Mono&quot;, Menlo, monospace; font-size: 13.6px; padding: 0.2em 0.4em; margin: 0px; background-color: var(--color-markdown-code-bg); border-radius: 6px;">GITEE_PASSWORD</code><span>&nbsp;</span>存放 Gitee 帐号的密码。</summary></details>

<details style="box-sizing: border-box; display: block; margin-top: 0px; margin-bottom: 16px;"><summary style="box-sizing: border-box; display: list-item; cursor: pointer;">3. 在 GitHub 的个人设置页面「<a href="https://github.com/settings/keys" style="box-sizing: border-box; background-color: initial; color: var(--color-text-link); text-decoration: none;">Settings -&gt; SSH and GPG keys</a>」 配置 SSH 公钥（即：<code style="box-sizing: border-box; font-family: SFMono-Regular, Consolas, &quot;Liberation Mono&quot;, Menlo, monospace; font-size: 13.6px; padding: 0.2em 0.4em; margin: 0px; background-color: var(--color-markdown-code-bg); border-radius: 6px;">id_rsa.pub</code>），命名随意。</summary></details>

<details style="box-sizing: border-box; display: block; margin-top: 0px; margin-bottom: 16px;"><summary style="box-sizing: border-box; display: list-item; cursor: pointer;">4. 在 Gitee 的个人设置页面「<a href="https://gitee.com/profile/sshkeys" rel="nofollow" style="box-sizing: border-box; background-color: initial; color: var(--color-text-link); text-decoration: none;">安全设置 -&gt; SSH 公钥</a>」 配置 SSH 公钥（即：<code style="box-sizing: border-box; font-family: SFMono-Regular, Consolas, &quot;Liberation Mono&quot;, Menlo, monospace; font-size: 13.6px; padding: 0.2em 0.4em; margin: 0px; background-color: var(--color-markdown-code-bg); border-radius: 6px;">id_rsa.pub</code>），命名随意。</summary></details>

如果一切配置正常，并成功触发 [Gitee Pages Action](https://github.com/marketplace/actions/gitee-pages-action) ，我们会在 Gitee 公众号收到一条登录通知。这是 Gitee Pages Action 程序帮我们登录到 Gitee 官网，并为我们点击了项目的部署按钮。

[![img](/img-post/开发/其它/Git/自动更新gitee pages.assets/wechat_notification.png)](https://github.com/yanglbme/gitee-pages-action/blob/main/images/wechat_notification.png)

## 错误及解决方案

| #    | 错误                                                         | 解决方案                                                     |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | Error: Wrong username or password, login failed .            | 帐号或密码错误，请检查参数 `gitee-username`、`gitee-password`是否准确配置。 |
| 2    | Error: Need captcha validation, please visit https://gitee.com/login, login to validate your account. | 需要图片验证码校验。可以手动登录 Gitee 官方，校验验证码。    |
| 3    | Error: Need phone captcha validation, please follow gitee wechat subscription and bind your account. | 需要短信验证码校验。可以关注 Gitee 微信公众号，并绑定 Gitee 帐号，接收登录提示。[#6](https://github.com/yanglbme/gitee-pages-action/issues/6) |
| 4    | Error: Do not deploy frequently, try again one minute later. | 短期内频繁部署 Gitee Pages 导致，可以稍后再触发自动部署。    |
| 5    | Error: Deploy error occurred, please check your input `gitee-repo`. | `gitee-repo` 参数格式如：`doocs/advanced-java`，并且严格区分大小写，请准确填写。[#10](https://github.com/yanglbme/gitee-pages-action/issues/10) |
| 6    | Error: Unknown error occurred in login method, resp: ...     | 登录出现未知错误，请在 [issues](https://github.com/yanglbme/gitee-pages-action/issues) 区反馈。 |
| 7    | Error: Rebuild page error, status code: xxx                  | 更新 Pages 时状态码异常，请尝试再次触发 Action 执行。也可能为 gitee pages 未初始化，第一次需要手动部署 gitee pages。 |
| 8    | Error: HTTPSConnectionPool(host='gitee.com', port=443): Read timed out. (read timeout=6) | 网络请求出错，请尝试 Re-run jobs 。[#27](https://github.com/yanglbme/gitee-pages-action/issues/27) |
| 9    | [git@github.com](mailto:git@github.com): Permission denied (publickey). fatal: Could not read from remote repository. Please make sure you have the correct access rights and the repository exists.. | SSH 公私钥配置有问题，或是使用了带密码的私钥，请参照上文提及的密钥配置步骤进行相应配置。[#29](https://github.com/yanglbme/gitee-pages-action/issues/29) |
| 10   | Hexo Gitee Pages 自动部署站点问题。                          | [@No5972](https://github.com/No5972) 详细给出了一种解决方案。[#34](https://github.com/yanglbme/gitee-pages-action/issues/34) |
| ...  | ...                                                          | ...                                                          |

注：

1. `branch` 参数默认是 `master`，如果你是部署在 `gh-pages`(或者 `main`) 分支等等，务必指定 `branch: gh-pages`(或者 `branch: main`)。

2. `branch` 对应的分支，必须在仓库中实际存在，请不要随意（不）指定分支，否则可能导致 Gitee Pages 站点出现 404 无法访问的情况。

3. 示例中触发 Action 执行的事件设置为

    

   ```
   page_build
   ```

   ，你也可以根据实际情况指定为其它的触发事件。如下：

   ```
   on:
     push:
       branches: [main, master]
   ```

   更多触发事件，请参考

    

   Events that trigger workflows

   。

## 谁在使用

| [![img](/img-post/开发/其它/Git/自动更新gitee pages.assets/antv.png) 蚂蚁金服 - 数据可视化](https://github.com/antvis) | [![img](自动更新gitee pages.assets/doocs.png) Doocs 技术社区](https://github.com/doocs) | [![img](自动更新gitee pages.assets/youzan.jpg) 有赞](https://github.com/youzan) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [antvis/g](https://github.com/antvis/g)[antvis/F2](https://github.com/antvis/F2)[antvis/G6](https://github.com/antvis/G6)[antvis/L7](https://github.com/antvis/L7)[antvis/G2Plot](https://github.com/antvis/G2Plot)[antvis/Graphin](https://github.com/antvis/Graphin)[antvis/antvis.github.io](https://github.com/antvis/antvis.github.io) | [doocs/jvm](https://github.com/doocs/jvm)[doocs/leetcode](https://github.com/doocs/leetcode)[doocs/advanced-java](https://github.com/doocs/advanced-java)[doocs/doocs.github.io](https://github.com/doocs/doocs.github.io)[doocs/source-code-hunter](https://github.com/doocs/source-code-hunter) | [youzan/vant](https://github.com/youzan/vant)[youzan/vant-weapp](https://github.com/youzan/vant-weapp) |

## License

[![FOSSA Status](/img-post/开发/其它/Git/自动更新gitee pages.assets/68747470733a2f2f6170702e666f7373612e636f6d2f6170692f70726f6a656374732f6769742532426769746875622e636f6d25324679616e676c626d6525324667697465652d70616765732d616374696f6e2e7376673f747970653d6c61726765)](https://app.fossa.com/projects/git%2Bgithub.com%2Fyanglbme%2Fgitee-pages-action?ref=badge_large)