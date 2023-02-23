---
tags:
    - NodeJS
---

使用grunt自动编译less文件并压缩css文件

首先创建 Gruntfile.js 和 package.json 两个文件：



Gruntfile.js:

module.exports = function (grunt) {



    // Project configuration.

    grunt.initConfig({

        pkg: grunt.file.readJSON('package.json'),

        less: {

            development: {

                files: [{

                    expand: true,

                    cwd: './static/less',

                    src: ['**/*.less'],

                    dest: './static/css',

                    ext: '.css'

                }]

            }

        },

        //css压缩插件

        cssmin: {

            target: {

                files: [{

                    expand: true,

                    cwd: './static/css',

                    src: ['**/*.css', '!**/*.min.css'],

                    dest: './static/css',

                    ext: '.min.css'

                }]

            }

        },

        watch: {

            client: {    //用于监听less文件,当改变时自动编译成css文件

                files: ['./static/less/**/*.less'],

                tasks: ['less'],

                options: {

                    livereload: true

                }

            },

            clientcss: {    //用于监听css文件,当改变时自动压缩css文件

                files: ['./static/css/*.css'],

                tasks: ['cssmin'],

                options: {

                    livereload: true

                }

            }

        }

    });



    grunt.loadNpmTasks('grunt-contrib-less');

    grunt.loadNpmTasks('grunt-contrib-cssmin');

    grunt.loadNpmTasks('grunt-contrib-watch');



    // Default task(s).

    grunt.registerTask('default', ['less', 'cssmin', 'watch']);

};





package.json:

{

  "name": "www.xxx.com",

  "version": "0.0.1",

  "license": "ISC"

}



然后执行以下命令安装grunt到本地：

sudo npm install grunt -g



再安装依赖包：

npm install grunt grunt-contrib-less grunt-contrib-cssmin grunt-contrib-watch --save-dev



最后执行grunt即可：

$ grunt

Running "less:development" (less) task

>> 6 stylesheets created.



Running "cssmin:target" (cssmin) task

>> 6 files created. 65.93 kB → 47.1 kB



Running "watch" task

Waiting...



坑：

使用registry.npm.taobao.org安装grunt-contrib-watch不成功，改用官方registry即可。

package.json的version不能是2016.11的格式，改成三位如：0.2016.11即可。



参考：

http://jingyan.baidu.com/article/870c6fc3110be5b03fe4be09.html

http://gruntjs.com/getting-started

https://www.oschina.net/code/snippet_2289011_52994



