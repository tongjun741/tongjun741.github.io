---
tags:
    - 其它
    - Mysql
---

[mysql] Cannot add foreign key constraint

在第一行之前增加：

set foreign_key_checks=0;



http://stackoverflow.com/questions/15534977/mysql-cannot-add-foreign-key-constraint

