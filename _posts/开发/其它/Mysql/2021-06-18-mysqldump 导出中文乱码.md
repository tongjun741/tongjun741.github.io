---
tags:
    - 其它
    - Mysql
---

mysqldump 导出中文乱码

  使用mysqldump的时候需要加上--default-character-set=utf8:



mysqldump -h 192.168.1.3 -uroot -p123456 --default-character-set=utf8  --add-drop-table --ignore-table=dbname.servers dbname --triggers=true | mysql dbname -uroot -p123456 --default-character-set=utf8





首先要做的是要确定你导出数据的编码格式，使用mysqldump的时候需要加上--default-character-set=utf8， 



例如下面的代码： 

复制代码代码如下:

mysqldump -uroot -p --default-character-set=utf8 dbname tablename > bak.sql







那么导入数据的时候也要使用--default-character-set=utf8： 



复制代码代码如下:

mysql -uroot -p --default-character-set=utf8 dbname < bak.sql





这样统一编码就解决了mysql数据迁移中的乱码问题了 



http://www.jb51.net/article/31615.htm

