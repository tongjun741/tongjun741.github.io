---
tags:
    - 操作系统
    - Linux
---

linux下编译c代码时error：undefined reference to sem_wait 解决方法之一

最近做助教遇到学生写生产者消费者的代码遇到问题，在帮他们解决问题的时候遇到如下问题：



当我在终端输入如下：

gcc -o consumer.out consumer.c



就会出现下面的错误：

consumer.c:(.text+0xc9): undefined reference to `sem_post'

/tmp/ccvFyPLL.o: In function `consumer':

consumer.c:(.text+0x108): undefined reference to `sem_wait'

consumer.c:(.text+0x1c6): undefined reference to `sem_post'

/tmp/ccvFyPLL.o: In function `main':

consumer.c:(.text+0x1fe): undefined reference to `sem_init'

consumer.c:(.text+0x21a): undefined reference to `sem_init'

consumer.c:(.text+0x23f): undefined reference to `pthread_create'

consumer.c:(.text+0x264): undefined reference to `pthread_create'

consumer.c:(.text+0x278): undefined reference to `pthread_join'

consumer.c:(.text+0x28c): undefined reference to `pthread_join'





这个是因为pthread并非Linux系统的默认库，编译时注意加上-lpthread参数，以调用链接库



我们再一次在终端输入：gcc -o consumer.out consumer.c -lpthread



此时编译正确



执行：在终端输入：./consumer.out 



http://blog.sina.com.cn/s/blog_68b4ec9b0101hzrs.html

