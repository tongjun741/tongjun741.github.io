---
tags:
    - MongoDB
---

MongoDB统计collection占用空间大小

以M为单位统计collection

```javascript
var collectionNames= db.getCollectionNames();  
for (var i = 0; i < collectionNames.length; i++) {     
  var coll = db.getCollection(collectionNames[i]);   
  var stats = coll.stats(1024 * 1024);   
  print(stats.ns, stats.storageSize);  
}  

```



https://www.jianshu.com/p/14368175e605

