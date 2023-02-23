---
tags:
    - 前端
---

JavaScript 获得代码行号和脚本文件名

var log = {

    info: function(arg){

        try {

            throw new Error();

        } catch (e) {

            alert("Stack:" + e.stack);

            var loc= e.stack.replace(/Error\n/).split(/\n/)[1].replace(/^\s+|\s+$/, "");

            alert("Location: "+loc+"");

        }

    }

};

 

function foo(){

    log.info(123);

}

 

foo();





https://unmi.cc/javascript-get-execution-line-file/

