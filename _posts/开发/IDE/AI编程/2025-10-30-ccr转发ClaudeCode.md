---
tags:
    - IDE
    - AI编程
---

改VSCode 扩展：

```
{
  ...
    "claude-code.selectedModel": "volcengine,deepseek-v3-1-terminus",
    "claude-code.environmentVariables": [
        {
            "name": "ANTHROPIC_BASE_URL",
            "value": "http://127.0.0.1:3456"
        },
        {
            "name": "ANTHROPIC_AUTH_TOKEN", // 注意这里是ANTHROPIC_AUTH_TOKEN，而不是ANTHROPIC_API_KEY
            "value": "sk-xxxx"
        },
    ],
  ...
}
```

参考：

https://github.com/musistudio/claude-code-router/issues/852

这里面的方法1没生效



全局配置文件：

```
C:\Users\用户名\.claude\settings.json
```



使用替代模型：

https://modelscope.csdn.net/68e774a0a6dc56200e8ff056.html