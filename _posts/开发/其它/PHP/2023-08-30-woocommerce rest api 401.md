---
tags:
    - 其它
    - PHP
---

本地环境没有开启https时会报401，简单的做法是将此代码添加到 function.php 来解决该问题：

```
add_filter( 'woocommerce_rest_check_permissions', 'my_woocommerce_rest_check_permissions', 90, 4 );

function my_woocommerce_rest_check_permissions( $permission, $context, $object_id, $post_type  ){
  return true;
}
```

有其它建议说可以使用 postman 尝试 Oauth 1.0，我没试过。

原因是https://woocommerce.github.io/woocommerce-rest-api-docs/?javascript#authentication-over-http



https://stackoverflow.com/a/66429685



test111
