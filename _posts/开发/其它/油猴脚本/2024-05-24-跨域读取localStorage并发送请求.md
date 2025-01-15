---
tags:
    - 其它
    - 油猴脚本
---

```
// ==UserScript==
// @name         自动测试
// @namespace    http://tampermonkey.net/
// @version      2024-05-24
// @description  try to take over the world!
// @author       You
// @match        https://*.aaa.com/*
// @icon         data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==
// @grant        GM_xmlhttpRequest
// @require      https://cdn.staticfile.org/jquery/3.4.1/jquery.min.js
// @require      file://D:\ccc\自动测试.js
// ==/UserScript==

```



```
// 将当前页面的localStorage传递给总页面
if (location.href.includes('x.aaa.com') || location.href.includes('y.aaa.com')) {
    window.top.postMessage({
        hostname: location.hostname,
        bbb: localStorage.bbb
    }, '*');
}

// 仅在总页面添加按钮
if (location.href.includes('z.aaa.com')) {
    $(document).ready(function () {
        setTimeout(addButton, 2000);
    });
}

let bbbList = {
    z: localStorage.bbb,
};

function addButton() {
    const appIframe = document.body.appendChild(document.createElement('iframe'));
    appIframe.style.display = 'none';
    appIframe.src = 'https://x.aaa.com';

    const channelIframe = document.body.appendChild(document.createElement('iframe'));
    channelIframe.style.display = 'none';
    channelIframe.src = 'https://y.aaa.com';

    let createButton = document.getElementById('startCreate');
    // console.log(createButton);
    if (createButton) {
        return;
    }

    let welcomeDiv = document.querySelector("div[class*=right]");

    // console.log(welcomeDiv);
    if (!welcomeDiv) {
        return;
    }

    //Add a button for the user
    var node = document.createElement("div");
    node.innerHTML = `<a id="startCreate">自动测试</a>`;
    welcomeDiv.prepend(node);

    window.addEventListener('message', (e) => {
        console.log('received message', e.data);
        bbbList[e.data.hostname] = e.data.bbb;
    });

    $("#startCreate").click(() => {
        console.log(bbbList);

        GM_xmlhttpRequest({
            url: "https://x.aaa.com/api/sssss",
            method: "GET",
            headers: {
                "Content-type": "application/x-www-form-urlencoded",
                "Origin": "https://x.aaa.com",
                "authorization": bbbList['x.aaa.com'],
            },
            onload: function (xhr) {
                console.log(xhr.responseText);
            }
        })
    });
}
```



https://www.cnblogs.com/Colincora/articles/16685466.html

https://stackoverflow.com/questions/41111556/how-can-i-achieve-cross-origin-userscript-communication