---
tags:
    - 前端
---

部分chrome版本中父元素display为flex时，子元素使用高度是0

原因是部分chrome版本中父元素display为flex时，子元素使用height: 100%后高度还是0。

解决办法是在父元素的样式增加：

flex-direction: column;

display: flex;

子元素的样式增加：

flex: 1;

 

参考：https://blog.hduzplus.xyz/articles/2017/05/07/1494162203713.html



```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=10.0">

    <title>font test</title>
    <style type="text/css">
        div {
            font-size: 14px;
        }

        @font-face {
            font-family: 'HanHei SC';
            src: url('https://s.aa.com/assets/font/HanHei-SC-text.woff') format('woff');
        }

        body {
            font-family: "HanHei SC", "Arial", sans-serif;
        }

        .left-part {
            position: absolute;
            top: 0px;
            bottom: 0;
            left: 0;
            right: 328px;
            display: flex;
            flex-direction: column;
        }
        .credential-host-list-table {
            flex: 1 0 auto;
            display: flex;
            flex-direction: column;
        }
        .table-body {
            flex: 1 1 auto;
            text-align: center;
            background-color: red;

            /*为了解决.nano的height设置为100%时高度还是0的问题*/
            flex-direction: column;
            display: flex;
        }
        .nano {
            /*为了解决.nano的height设置为100%时高度还是0的问题*/
            flex: 1;

            position: relative;
            width: 100%;
            height: 100%;
            overflow: hidden;
            -webkit-overflow-scrolling: touch;

            background-color: green;
        }
    </style>
</head>
<body>
<div class="left-part">
    <div class="credential-host-list-table">
        <div class="table-body">
            <div class="nano has-scrollbar">
                <div class="nano-content" tabindex="-1" style="right: -8px;">

                </div>
            </div>
        </div>
    </div>
</div>
</body>
</html>

```



