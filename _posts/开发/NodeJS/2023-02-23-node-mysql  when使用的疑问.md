---
tags:
    - NodeJS
---

node-mysql  when使用的疑问

Node.js Async.js结合node-mysql使用的疑问



https://segmentfault.com/q/1010000004180541



使用Async.js查询三条SQL语句并返回结果：

var sqls = {
  table_a: "select count(*) from table_a",
  table_b: "select count(*) from table_b",
  table_c: "select count(*) from table_c"
};

async.map(sqls, function(item, callback) {
  connection.query(item, function(err, results) {
    callback(err, results);
  });
}, function(err, results) {
  if(err) {
    console.log(err);
  } else {
    console.log(results);
  }
});

可以输出：

{ table_a: [ { 'count(*)': 26 } ],
  table_b: [ { 'count(*)': 3 } ],
  table_c: [ { 'count(*)': 2 } ] }

根据map函数的文档：

arr - An array to iterate over.

iterator(item, callback) - A function to apply to each item in arr. The iterator is passed a callback(err, transformed) which must be called once it has completed with an error (which can be null) and a transformed item.

callback(err, results) - Optional A callback which is called when all iterator functions have finished, or an error occurs. Results is an array of the transformed items from the arr.

connection.query函数符合第二个参数iterator(item, callback)。

为什么以下的使用方式会报错：

var sqls = {
  table_a: "select count(*) from table_a",
  table_b: "select count(*) from table_b",
  table_c: "select count(*) from table_c"
};

async.map(sqls, connection.query, function(err, results) {
  if(err) {
    console.log(err);
  } else {
    console.log(results);
  }
});

TypeError: Cannot read property 'typeCast' of undefined

- 2015年12月22日提问

 

- 编辑

 

- 评论

 

- 邀请回答

 

- 更多

默认排序时间排序

1个回答

0

采纳

正确代码如下：

async.map(sqls, connection.query.bind, function(err, results) {
  if(err) {
    console.log(err);
  } else {
    console.log(results);
  }
});

原因是connection.query运行中的 this 并不是指向 connection, 所以会导致错误，官方对此有解释https://github.com/caolan/async#binding-a-context-to-an-iterator

- 2月1日回答

 

- 编辑

 

- 评论



