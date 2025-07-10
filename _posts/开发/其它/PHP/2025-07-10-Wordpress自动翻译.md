---
tags:
    - 其它
    - PHP
---

安装插件：Loco Translate

开通gcp的 Cloud Translation API ，并创建凭据

进入wordpress后台左侧菜单的“Loco Translate”，在“设置”=》“API密钥”配置Loco Translate中的Google翻译密钥。

点击左侧菜单的“主题”/"插件"，找到要翻译的项目，新建语言，点击编辑，再点同步读取代码中要翻译的项目。

点击自动，选择“Google Translate”和“覆盖现有翻译”，再点翻译。

翻译完成后点击保存。



如果源语言不是英语，可以修改源代码wp-content/plugins/loco-translate/pub/js/min/admin.js  第5068行。

```
url: "https://translation.googleapis.com/language/translate/v2?source=" + n + "&target=" + r + "&format=" + b,

改成：

url: "https://translation.googleapis.com/language/translate/v2?source=zh&target=" + r + "&format=" + b,
```



