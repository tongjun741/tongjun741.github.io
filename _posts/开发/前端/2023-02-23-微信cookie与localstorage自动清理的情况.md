---
tags:
    - 前端
---

微信cookie与localstorage自动清理的情况

android(小米平板):

    kill进程都不清理

    退出微信账号重新登录后都被清理

ios(6s):

    kill进程都不清理

    退出微信账号重新登录后只有cookie被清理,localstorage是保留的



```
<!doctype html>


<head>


    <meta charset="utf-8">


    <title>Webpack App</title>

    <link rel="shortcut icon" href="./common/favicon.ico">

    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />

    <meta name="viewport" content="width=device-width,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">


    <meta name="format-detection" content="telephone=no" />

    <meta name="renderer" content="webkit">


</head>

<body class="body-cls">

now:<div id="now"></div>

last cookie:<div id="cookie"></div>

last storage:<div id="storage"></div>



</body>

    <script>


    window.onload = function(){

        var now = new Date().toString();

            document.getElementById('now').innerHTML  = now;

        if(document.cookie){

            document.getElementById('cookie').innerHTML  = document.cookie;

        }

            document.cookie = 'x='+now+";expires=" + new Date('2999-01-01').toGMTString();

        if(localStorage && localStorage.a){

            document.getElementById('storage').innerHTML  = localStorage.a;

        }

            localStorage.a = now;

    };

    </script>

</html>
```



