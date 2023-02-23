---
tags:
    - Python
---

Python多进程全局共享counter（multiprocessing包实现）

Python代码

```
# -*- coding: UTF-8 -*-   
  
from multiprocessing import Pool, Lock, Value  
import os  
  
tests_count = 80  
  
lock = Lock()  
  
counter = Value('i', 0) # int type，相当于java里面的原子变量  
  
  
def run(fn):  
    global tests_count, lock, counter  
    with lock:  
        counter.value += 1  
  
    print 'NO. (%d/%d) test start. PID: %d ' %(counter.value, tests_count,  os.getpid())  
    # do something below ...  
  
  
if __name__ == "__main__":  
    pool = Pool(10)  
    # 80个任务，会运行run()80次，每次传入xrange数组一个元素  
    pool.map(run, xrange(80))  
    pool.close()  
    pool.join()  
```
    
输出：
```
NO. (1/80) test start. PID: 23785 
NO. (2/80) test start. PID: 23785 
NO. (3/80) test start. PID: 23786 
NO. (5/80) test start. PID: 23786 
...
NO. (77/80) test start. PID: 23785 
NO. (78/80) test start. PID: 23791 
NO. (79/80) test start. PID: 23786 
```

注意事项：
如果使用multiprocessing包，则要按照它的套路走，标准库threading包里面threading.Lock()、acquire()、release()这几个同步方法不生效了，因为Pool是多进程的。上面code可以直接run

http://heipark.iteye.com/blog/2148737