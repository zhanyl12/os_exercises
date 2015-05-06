某寺庙，有小和尚、老和尚若干．庙内有一水缸，由小和尚提水入缸，供老和尚饮用。水缸可容纳10桶水，每次入水、取水仅为1桶，不可同时进行。水取自同一井中，水井径窄，每次只能容纳一个水桶取水。设水桶个数为3个，试用信号灯和PV操作给出老和尚和小和尚的活动。

> 信号量问题

```
from threading import Thread, Condition
import time
import random
 
queue = []
MAX_NUM = 10
condition = Condition()
 
class ProducerThread(Thread):
    def run(self):
        nums = range(3)
        global queue
        while True:
            condition.acquire()
            if len(queue) == MAX_NUM:
                print "well full, xiaoheshang is waiting"
                condition.wait()
                print "Space in well,xiaoheshang can add water"
            num = random.choice(nums)
            queue.append(num)
            print "Add", num
            condition.notify()
            condition.release()
            time.sleep(random.random())
 
class ConsumerThread(Thread):
    def run(self):
        global queue
        while True:
            condition.acquire()
            if not queue:
                print "Nothing in well, laoheshang is waiting"
                condition.wait()
                print "xiaoheshang added something to the well"
            num = queue.pop(0)
            print "Drink", num
            condition.notify()
            condition.release()
            time.sleep(random.random())
 
ProducerThread().start()
ConsumerThread().start()
```

> 条件变量实现

```
#coding=utf-8
#!/usr/bin/env python

import threading  
import time  

condition = threading.Condition()  
products = 0  

class Producer(threading.Thread):  
    def __init__(self):  
        threading.Thread.__init__(self)  

    def run(self):  
        global condition, products  
        while True:  
            if condition.acquire():  
                if products < 10:  
                    products += 1;  
                    print "xiaoheshang(%s):deliver one, now products:%s" %(self.name, products)  
                    condition.notify()  
                else:  
                    print "xiaoheshang(%s):already 10, stop deliver, now products:%s" %(self.name, products)  
                    condition.wait();  
                condition.release()  
                time.sleep(1)  

class Consumer(threading.Thread):  
    def __init__(self):  
        threading.Thread.__init__(self)  

    def run(self):  
        global condition, products  
        while True:  
            if condition.acquire():  
                if products > 1:  
                    products -= 1  
                    print "laoheshang(%s):consume one, now products:%s" %(self.name, products)  
                    condition.notify()  
                else:  
                    print "laoheshang(%s):only 1, stop consume, products:%s" %(self.name, products)  
                    condition.wait();  
                condition.release()  
                time.sleep(2)  

if __name__ == "__main__":  
    for p in range(0, 3):  
        p = Producer()  
        p.start()  

    for c in range(0, 3):  
        c = Consumer()  
        c.start() 
```
