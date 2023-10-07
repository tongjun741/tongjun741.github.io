---
tags:
    - 其它
    - PHP
---

```
add_filter( 'register_block_type_args', 'core_image_block_type_args', 10, 3 );
function core_image_block_type_args( $args, $name ) {
    if ( $name == 'core/image' ) {
        $args['render_callback'] = 'modify_core_image';
    }
    return $args;
}

function modify_core_image($attributes, $content) {
  	// Modify core image return content or something new here
	return $content;
}
```



https://ilikekillnerds.com/2022/04/how-to-override-wordpress-gutenberg-core-blocks-output/