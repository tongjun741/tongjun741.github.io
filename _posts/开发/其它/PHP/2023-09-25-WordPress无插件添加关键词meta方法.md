---
tags:
    - 其它
    - PHP
---

这篇文章继续讨论**无插件**为Wordpress添加meta的技术方案，讨论如何使用**自定义字段**来添加关键词。



按照[将tags转化为关键词的解决方案](https://magichao.com/blog/2015/03/wordpress-add-meta/)处理后，为了真正达到优化搜索引擎的目的，需要**按照keywords的思路**给文章设置tags标签。结果就形成了如下图的超多标签。

![将tags标签设为网页关键词可能会造成混乱的场面](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-混乱的标签.jpg)以keywords的思路设置标签时出现的混乱局面。图片来自[美哉设计工作室](http://www.magi-design.com/chaos/)早期版本截图。

这不但影响网页的简洁美观，更重要的是，**破坏了标签tags的基本交互功能**——用户无法通过点击标签获得与之相关的其它文章——进而降低了用户交互体验的舒适度。

其根本原因，从设计的角度来说，在于标签和关键词的**功能**和面对**对象**不同。

tags标签属于服务于**人类**阅读的**分类方法**，其存在初衷是为了**将同一网站内的涉及同一主题或具有某个相同属性的内容联系起来**。在Wordpress网站内容组织中，标签可被视为分类目录的补充，是不同组织结构的体现。

而keywords关键词则服务于**检索**，其存在初衷是为了**帮助人类从浩瀚的信息库中，更快地找到需要的信息**。在互联网活动中，网页的keywords关键词属性更多供**搜索引擎阅读**，而非人类。在现在网站建设中，keywords的作用在于，当用户在搜索某些信息时，帮助搜索引擎判断网站是否与用户所搜索的信息相关以及相关程度，从而觉得是否呈现某一网站的内容以及搜索结果的排序。

因此，如[前文](https://magichao.com/blog/2015/03/wordpress-add-meta/)所述，如果贵站的网站结构简单、内容不多，不需要多种维度的站内内容管理，则使用文章的tags属性充当页面keywords关键词的方案完全足够；但如果贵站的内容具有不同的分类方式，需要使用tags属性进行导航，则前述方法，会带来不必要的混乱，且降低用户体验。这种情况下，我们就要考虑使用**自定义栏目**（新版翻译为“自定义字段”）来实现keywords关键词设置的方案。

自定义栏目/字段Custom Field是Wordpress内置的功能。**通过自定义栏目/字段，我们可以为文章添加属性或内容**。许多主题或插件也是通过自定义栏目/字段来实现各种功能的。

在默认状态下，自定义栏目/字段并不显示，**需要我们手动调出**。在经典编辑器和古腾堡编辑器中，调出方法略不同。

#### 经典编辑器的调出方法

1 在编辑文章的页面右上角找到并点击【显示选项】。

![在Wordpress经典编辑器中显示自定义字段的方法-step1-点击右上角”自定义选项“](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-经典编辑器显示选项位置.jpg)显示选项按钮的位置

2 勾选【自定义栏目】。

![在Wordpress经典编辑器中显示自定义字段的方法-step2-勾选“自定义字段"](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-经典编辑器勾选自定义字段.jpg)勾选自定义栏目

#### 古腾堡编辑器的调出方法

1 在编辑文章的页面右上角找到并点击汉堡菜单按钮。

![在Wordpress古腾堡编辑器显示自定义字段-step1-汉堡菜单按钮](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-古腾堡编辑器汉堡按钮.jpg)汉堡菜单按钮

2 在弹出的菜单中，点击【选项】

![在Wordpress古腾堡编辑器显示自定义字段-step2-点击“选项”](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-古腾堡编辑器选项.jpg)【选项】在菜单最末位置

3 在弹出的对话框中勾选“自定义字段”

![在Wordpress古腾堡编辑器显示自定义字段-step3-勾选“自定义字段”](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-古腾堡编辑器自定义字段custom-field.jpg)勾选“自定义字段”

上述操作完成后，在文章编辑区下方会出现**自定义栏目/字段的编辑区**。有的主题会自带一些自定义栏目，这种情况下**需要我们先点击【输入新栏目】**，再输入信息；如果没有自带信息，则我们可以**直接输入**。

![使用自定义字段为Wordpress页面添加关键词的演示-step1-点击“添加自定义字段”](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-添加字段.jpg)输入新的自定义栏目

我们在**左侧输入自定义栏目的键值Key**，**右侧输入值Value**。在这里，我们将键值Key设为keywords，值Value为我们想为这篇文章添加的关键词。输入后点击添加即可。如下图所示。

![使用自定义字段为Wordpress页面添加关键词的演示-step2-添加值和关键词](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-添加关键词.jpg)输入自定义栏目的键值和值

其中， “keywords”可以是**任意字符串**，只要满足：a自己能记住、b不与已存在的值相同，这两个条件即可。

上述操作，会在数据库中建立一个新的键值对来保存我们输入的信息。下面**在header.php文件中调用这个值**即可。找到[[优化版](https://magichao.com/blog/2015/03/wordpress-add-meta/)]中添加的那段代码，将`$keywords = `之后的内容删除，改为`get_post_meta($post->ID, 'keywords', true)`，并删掉之后的三行代码。

结果如下图。

![img](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-修改获取关键词的方法.jpg)修改后的代码

如果输入了其它字符串作为关键词的key键，则 “ $keywords = get_post_meta($post->ID, ‘keywords’, true) ”中的后一个’keywords’**要改为自己设置的字符串**。

然后正常设置文章的标签，【更新】并【查看】文章。

![修改后的页面显示tags标签的效果](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-修改后的页面显示.jpg)修改后，可以正常设置标签

右键点击页面，选择【查看源代码】（不同浏览器措辞不同），能看到在keywords的meta中，显示的是我们键入的内容。

![img](/img-post/开发/其它/PHP/WordPress无插件添加关键词meta方法.assets/magichao-修改后的源代码.jpg)网页添加正确的关键词keywords微数据

最终代码如下，添加代码的位置和方法详见[WordPress无插件添加关键词meta方法[优化版\]](https://magichao.com/blog/2015/03/wordpress-add-meta/)一文。

```
 <?php
 /*
 *将摘要变成描述，将标签变成关键词的代码；放在header文件title之后。
 */
 if (is_home() or is_front_page()){
 $keywords = "首页关键词";
 $description = "首页描述";
 } elseif (is_single()){
 if ($post->post_excerpt) {
 $description = $post->post_excerpt;
 } else {
 $description = substr(strip_tags($post->post_content),0,220);
 }
 $keywords = get_post_meta($post->ID, 'keywords', true);
 }
 }
 ?>
 <meta name="keywords" content="通用关键词 <?=$keywords?>" />
 <meta name="description" content="通用关键词<?=$description?>" />
```

需要说明的是，添加自定义字段键值对的操作，**只需进行一次**。之后全站的文章都会具有这个属性，**只需为不同文章添加不同的值即可**。

关于自定义栏目Custom Field的更多信息请阅读[WordPress的官网介绍](https://wordpress.org/support/article/custom-fields/)（英文）。

参考资料
https://wordpress.org/support/article/custom-fields/
https://wordpress.org/support/article/posts-tags-screen/
https://wordpress.org/support/article/taxonomies/





https://magichao.com/blog/2015/04/wordpress-add-meta2/