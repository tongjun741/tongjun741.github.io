---
tags:
    - Java
---

Linux中jar,war包的解压、修改与重新打包

## 解压

实验中使用远程操作服务器进行jar包的解压，使用的指令为：jar -xvf **.jar。

## 文件操作

在文件夹中找到需要修改或替换的文件，直接进行相关操作。  

## 压缩

（此处遇到问题较多）：jar指令的相关参数如下

```
Usage: jar {ctxui}[vfmn0PMe] [jar-file] [manifest-file] [entry-point] [-C dir] files ...
Options:
    -c  create new archive
    -t  list table of contents for archive
    -x  extract named (or all) files from archive
    -u  update existing archive
    -v  generate verbose output on standard output
    -f  specify archive file name
    -m  include manifest information from specified manifest file
    -n  perform Pack200 normalization after creating a new archive
    -e  specify application entry point for stand-alone application
        bundled into an executable jar file
    -0  store only; use no ZIP compression
    -P  preserve leading '/' (absolute path) and ".." (parent directory) components from file names
    -M  do not create a manifest file for the entries
    -i  generate index information for the specified jar files
    -C  change to the specified directory and include the following file
If any file is a directory then it is processed recursively.
The manifest file name, the archive file name and the entry point name are
specified in the same order as the 'm', 'f' and 'e' flags.

Example 1: to archive two class files into an archive called classes.jar:
       jar cvf classes.jar Foo.class Bar.class
Example 2: use an existing manifest file 'mymanifest' and archive all the
           files in the foo/ directory into 'classes.jar':
       jar cvfm classes.jar mymanifest -C foo/ .


```

实验中涉及的jar包是一个完整的springboot项目，所以需要最后打包的jar包是一个可执行文件，因此需要指定manifest.mf文件。

### 注意事项

最终的打包指令为：`jar cvfM0 **.jar ./`其中./表示当前目录下所有的文件进行打包，

切记，指令中M后边必须添加0。-0 仅存储; 不使用任何 ZIP 压缩，这样可以保证使用解压之前的manifest.mf文件。

[https://yangxx.net/?p=3745](https://yangxx.net/?p=3745)