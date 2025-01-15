---
tags:
    - 其它
    - Git
    - GitHub Actions
---

## 将 webhook 触发器添加到 GitHub Action

让我们在现有触发器之后添加一个部分，这将允许我们使用 Webhook 通知启动工作流程：

```yaml
name: Windows10

on:
  repository_dispatch:
    types: [win10-start]
    
  workflow_dispatch:
    inputs:
      software_download_url:
        description: 'Download url'
        required: true

jobs:

  Windows10:
    runs-on: windows-latest
    
    steps:
      - uses: actions/checkout@v2

      - name: Use Node.js 20
        uses: actions/setup-node@v2
        with:
          node-version: 20
          #cache: 'yarn'
```



要测试调用此触发器，您需要向以下 URL 发送特殊的 POST 请求：

```
https://api.github.com/repos/{owner}/{repo}/dispatches
```

该请求还需要包含以下标头：

```javascript
"Accept": "application/vnd.github+json"
"Authorization": "token {personal token with repo access}"
```

您需要提供具有访问范围的[个人 GitHub 令牌](https://github.com/settings/tokens?type=beta)`repo`。如果您的触发器还具有类型，请在请求正文中指定它：

```yaml
{"event_type": "webhook"}
```

## 使用curl触发

```
curl -X POST \
  -H "Authorization: token {githubtoken}" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/{owner}/{repo}/dispatches/actions/workflows/Windows10.yml/dispatches \
  -d '{"ref": "main", "inputs": {"software_download_url": "https://xxx.com/aa.exe"}}'

```



https://kontent.ai/blog/how-to-trigger-github-action-using-webhook-with-no-code/
