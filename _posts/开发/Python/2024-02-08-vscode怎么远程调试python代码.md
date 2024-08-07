---
tags:
    - Python
---

远程调试Python代码在VSCode中是一项强大的功能。本文将指导您如何设置VSCode来进行远程Python代码调试：1. 准备VSCode和Python环境；2. 安装必要的VSCode扩展；3. 配置远程服务器；4. 创建调试配置；5. 执行远程调试。通过本文，您将能够轻松掌握VSCode的远程调试功能，优化Python代码开发工作流程。

## 1.准备VSCode和Python环境

首先，确保您的本地机器已安装VSCode和Python。VSCode提供了一个灵活的集成开发环境，而Python是我们要进行远程调试的语言。

## 2.安装必要的VSCode扩展

为了实现Python的远程调试，我们需要在VSCode中安装两个扩展：Python 和 Remote – SSH。这两个扩展可以从VSCode的扩展市场中找到并安装。

## 3.配置远程服务器

远程服务器上需要有Python环境和适当的调试库。一般情况下，我们使用 ptvsd 作为Python的远程调试库。您可以使用pip来安装它：

```
pip install ptvsd
```

在Python代码中，您需要插入以下代码片段来启动远程调试服务器：

```
import ptvsd
ptvsd.enable_attach(address=('0.0.0.0', 5678))
ptvsd.wait_for_attach()
```

## 4.创建调试配置

在VSCode中，点击左侧的调试图标，然后点击齿轮图标创建一个新的调试配置。选择Python并创建一个远程连接。配置内容如下：

```
{
    "name": "Python: Remote Attach",
    "type": "python",
    "request": "attach",
    "connect": {
        "host": "远程服务器IP",
        "port": 5678
    },
    "pathMappings": [{
        "localRoot": "${workspaceFolder}",
        "remoteRoot": "/path/to/your/remote/directory"
    }]
}
```

5.执行远程调试

首先，运行远程服务器上的Python代码。当代码执行到 ptvsd.wait_for_attach() 时，它会等待调试器连接。此时，在VSCode中点击调试按钮开始远程调试。此时，您可以设置断点、检查变量和执行代码，就像在本地进行调试一样。



https://docs.pingcode.com/ask/59048.html