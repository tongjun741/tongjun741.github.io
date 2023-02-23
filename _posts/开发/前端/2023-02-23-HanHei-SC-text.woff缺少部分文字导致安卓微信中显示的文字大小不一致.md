---
tags:
    - 前端
---

HanHei-SC-text.woff缺少部分文字导致安卓微信中显示的文字大小不一致





<!DOCTYPE html>

<html>

<head>

    <meta charset="utf-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=10.0">



    <title>font test</title>

<style type="text/css">

    div{

        font-size: 14px;

    }



    @font-face {

        font-family: 'HanHei SC';

        src: url('https://s.aaa.com/assets/font/HanHei-SC-text.woff') format('woff');

    }



    body {

        font-family: "HanHei SC","Arial",sans-serif;

    }

</style>

</head>

<body >

<div>主机查看有的冤枉</div>

</body>

</html>

