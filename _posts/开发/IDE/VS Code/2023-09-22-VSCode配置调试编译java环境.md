---
tags:
    - IDE
    - VS Code
---

# 1、安装插件：Extension Pack for Java

# 2、vsCode 配置Java:home

在 VSCode 里，依次打开: 文件 -> 首选项 -> 设置，然后输入 javahome 进行搜索 点击在setting.json中编辑

![在这里插入图片描述](/img-post/开发/IDE/VS Code/VSCode配置调试编译java环境.assets/b5cf7f5ac4ecae741d0fc5adb04de5c0.png)

在这里插入图片描述

 增加"java.home"项 注意修改为自己的JDK安装路径 

![在这里插入图片描述](/img-post/开发/IDE/VS Code/VSCode配置调试编译java环境.assets/f1e7606c5bcd3185fe813954bea1e374.png)

在这里插入图片描述

 下面附上本人的setting.json配置

```javascript
{

    "window.zoomLevel": 1,
    "java.home": "C:\\software\\jdk1.8\\jdk1.8.0_111",
    "java.semanticHighlighting.enabled": true,
    "editor.suggestSelection": "first",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "java.configuration.checkProjectSettingsExclusions": false,
    "git.ignoreWindowsGit27Warning": true,
    "java.requirements.JDK11Warning": false,

}
```



# 3.最后，调试试运行

运行测试类两种方式 

![在这里插入图片描述](/img-post/开发/IDE/VS Code/VSCode配置调试编译java环境.assets/c42743046f13e6536c38fb4350444df0.png)

```
public class App {

  public static void main(String[] args) {
    System.out.println("Hello vscode!");
  }

}
```





https://cloud.tencent.com/developer/article/2171197
