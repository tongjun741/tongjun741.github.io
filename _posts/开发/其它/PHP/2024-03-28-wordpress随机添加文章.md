---
tags:
    - 其它
    - PHP
---

```
for ($i=0;$i<100;$i++){
	// 创建一个新的文章
    $post = array(
        'post_title' => '随机文章标题', // 替换为你想要的标题
        'post_content' => '这里是文章内容', // 替换为你想要的内容
        'post_status' => 'publish',
        'post_author' => 1, // 文章作者的用户 ID
        'post_category' => array(84), // 指定分类的 ID
    );

    // 插入文章到数据库
    $post_id = wp_insert_post($post);

    if ($post_id) {
        echo '文章已成功添加，并且已指定到分类 ID 84';
    } else {
        echo '文章添加失败';
    }

}
```

