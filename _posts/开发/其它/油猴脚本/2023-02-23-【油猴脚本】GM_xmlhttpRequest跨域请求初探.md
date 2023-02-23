---
tags:
    - 其它
    - 油猴脚本
---

https://blog.csdn.net/chirenshuomeng1/article/details/103011165

# 缘起

公司叫我们去考金蝶，并要求认证。金蝶官网提供了一些试题，基本上每个视频学习完成后就有对应的试题测验一下。
观察后发现，金蝶的网页做得还是很有规律的，试题与视频都是一一对应的：
视频页面（教学视频）：
http://cplatform.kingdee.com/templates/course/course_details.html?courseId=**677**
测验页面（与视频对应的试题）：
http://cplatform.kingdee.com/templates/course/exam_formal.html?courseId=**677**
于是在查看这些试题的时候想把题目上传到自己的数据库里，再做个随机抽题的页面，不就可以很方便地自己刷题了吗？
说干就干——

# 试题抓取

这里，我们在浏览测验页面时使用油猴（tamperMonkey）自动运行js脚本。由于这不是本文的重点，直接上代码：

```javascript
//形如http://cplatform.kingdee.com/templates/course/exam_formal.html?courseId=677的测验页
    if(location.href.indexOf('exam_formal')>-1){
        let reg2 = /,/g
        let preCount =0, count = 0;
        const id = location.href.match(reg)[1];
        $.get('http://cplatform.kingdee.com/queryQuestionByCourseId?id=' + id, function(res){
            if(res.rows){
                for(let i = 0; i < res.rows.length; i++) {
                    let myAnswer = '';
                    let myOptions =[];
                    const options = res.rows[i].optionList;
                    let answer = '';
                    for (let j = 0; j < options.length; j++) {
                        myOptions.push(options[j].content.replace(reg2, '，'));
                        myAnswer += (options[j].isQuestionAnswer === 1) ? ((answer ? ',' : '') + String.fromCharCode(65 + j)) : '';
                    }
                    GM_xmlhttpRequest({
                        method: "post",
                        url: 'http://192.168.0.109:8080/subject',
                        data: 'typeName=【' + id + '】' + res.data.title + '&content=' +res.rows[i].title + '&answer='+myAnswer+'&options=' +JSON.stringify(myOptions),
                        headers:  {
                            "Content-Type": "application/x-www-form-urlencoded"
                        },
                        onload: function(res){
                            if(res.status === 200){
                                console.log('成功')
                            }else{
                                console.log('失败')
                                console.log(res)
                            }
                        },
                        onerror : function(err){
                            console.log('error')
                            console.log(err)
                        }
                    });
                }
        // 用于在页面上显示参考答案
        $('.col-xs-12').bind('DOMSubtreeModified', function(e){
            count++;
            setTimeout(function(){
                if(preCount !== count){
                    preCount = count;
                }else{
                    $('.col-xs-12').unbind('DOMSubtreeModified');
                    $('.look_answer_exam').show();
                }
            },100);
        });
    }
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
```

首先，利用金蝶的api获取题目及答案

```javascript
$.get('http://cplatform.kingdee.com/queryQuestionByCourseId?id=' + id)
1
```

这里的`id`就是http://cplatform.kingdee.com/templates/course/exam_formal.html?courseId=677中的`677`
请求得到的数据（在chrome的开发者工具中查看）如下：
![试题数据](/img-post/开发/其它/油猴脚本/【油猴脚本】GM_xmlhttpRequest跨域请求初探.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoaXJlbnNodW9tZW5nMQ==,size_16,color_FFFFFF,t_70.jpeg)
红框中的是题目提供的几个选项，如果该选项的`isQuestionAnswer`为1则表示该项是正确答案，`content`为选项的正文。

# 问题

金蝶的域名为 *http://cplatform.kingdee.com*
而我自己要上传数据的接口域名为 *http://192.168.0.109:8080*
显然，若使用ajax请求数据时会因跨域操作而被禁止。

```javascript
$.ajax({
                        method: "post",
                        url: 'http://192.168.0.109:8080/subject',
                        ...
                        onload: function(res){
                            if(res.status === 200){
                                console.log('成功')
                            }else{
                                console.log('失败')
                                console.log(res)
                            }
                        },
                        onerror : function(err){
                            console.log('error')
                            console.log(err)
                        }
                    });
                })
123456789101112131415161718
```

![跨域导致的请求失败](/img-post/开发/其它/油猴脚本/【油猴脚本】GM_xmlhttpRequest跨域请求初探.assets/20191111161000309.jpg)
数据在上传到自己的服务器之前就被拦截了。
为了快点解决问题，直接使用`tamperMonkey`的`GM_xmlhttpRequest`。

# GM_xmlhttpRequest用法

### 设置

在js脚本顶部添加以下代码以允许跨域并启用`GM_xmlhttpRequest`：

```javascript
// @grant        GM_xmlhttpRequest
// @connect      *
12
```

“@connect *”表示允许任何域名的跨域请求。当然，我们这里写成“@connect 192.168.0.109”也是可以的，注意不需要加`http://`！[1](https://blog.csdn.net/chirenshuomeng1/article/details/103011165#fn1)
关于的用法，最初我参考了tamperMonkey的文档[2](https://blog.csdn.net/chirenshuomeng1/article/details/103011165#fn2)
由于文档并未详细说明`data`的格式于是想当然的认为和`$.ajax`一样，代码如下：

```javascript
GM_xmlhttpRequest({
    method: "post",
    url: 'http://192.168.0.109:8080/subject',
    data: {
     	typeName: 'somthing',
     	content: 'somthing',
     	options: 'somthing'
	},
	onload: function(res){
		// code
	}
});
123456789101112
```

最后折腾了一个下午，发现Greasemonkey的文档[3](https://blog.csdn.net/chirenshuomeng1/article/details/103011165#fn3)才有正确的说明，当然，Greasemonkey用的是`GM.xmlHttpRequest`，因为我用的是Tampermonkey，故代码如下：

```javascript
GM_xmlhttpRequest({
    method: "post",
    url: 'http://192.168.0.109:8080/subject',
    data: 'typeName=XXX&content=XXX&options=XXX',
    headers: { "Content-Type": "application/x-www-form-urlencoded" },
    onload: function(r) {
    	// code
    }
});
123456789
```

即，将Object类型的data用字符串拼成一个字符串即可。

------

1. 可参阅：[tamperMonkey官方文档 - @connect](http://www.tampermonkey.net/documentation.php?ext=dhdg#_connect) [↩︎](https://blog.csdn.net/chirenshuomeng1/article/details/103011165#fnref1)
2. 见此处： [tamperMonkey官方文档 - @GM_xmlhttpRequest](http://www.tampermonkey.net/documentation.php?ext=dhdg#GM_xmlhttpRequest) [↩︎](https://blog.csdn.net/chirenshuomeng1/article/details/103011165#fnref2)
3. 见此处： [Greasemonkey的请求示例](https://wiki.greasespot.net/GM.xmlHttpRequest#Notes) [↩︎](https://blog.csdn.net/chirenshuomeng1/article/details/103011165#fnref3)