---
tags:
    - 其它
    - PHP
---

我们可以使用 “升级安全” 的方法覆盖这些文件，只需要复制 `templates` 目录中的文件到主题的 `woocommerce` 目录中即可，文件目录结构保持改变。如果我们当前使用的主题中没有 woocommerce 目录，新建一个即可。



示例：

wp-content/plugins/woocommerce/templates/single-product/review-meta.php

wp-content/themes/astra/woocommerce/single-product/review-meta.php



https://www.wpzhiku.com/overwrite-woocommerce-template-in-themes/

