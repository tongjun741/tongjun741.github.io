---
tags:
    - 其它
    - PHP
---

PHP DOMDocument保存xml时中文出现乱码的解决办法：

第一种：在loadHTML的时候指定编码，下面这段代码引用自php.net官方文档中的回复

**复制

```
$doc = new DOMDocument();
$doc->loadHTML('<?xml encoding="UTF-8">' . $html);
 
// dirty fix
foreach ($doc->childNodes as $item)
    if ($item->nodeType == XML_PI_NODE)
        $doc->removeChild($item); // remove hack
$doc->encoding = 'UTF-8'; // insert proper
```

第二种方法，通过iconv对输出的字符重新转换，代码如下：

**复制

```
echo iconv("UTF-8", "GB18030//TRANSLIT", $dom->saveXML($n) );
```





https://www.west.cn/docs/148765.html