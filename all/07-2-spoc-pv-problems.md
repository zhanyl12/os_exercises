某寺庙，有小和尚、老和尚若干．庙内有一水缸，由小和尚提水入缸，供老和尚饮用。水缸可容纳10桶水，每次入水、取水仅为1桶，不可同时进行。水取自同一井中，水井径窄，每次只能容纳一个水桶取水。设水桶个数为3个，试用信号灯和PV操作给出老和尚和小和尚的活动。

> 信号量问题

```
#include <stdio.h>
#include <windows.h>


#define BUFFER_SIZE 10
int buffer[BUFFER_SIZE];

unsigned int    in = 0;
unsigned int    out = 0;
typedef int Semaphore;
Semaphore   full = 0;
Semaphore   empty = BUFFER_SIZE;
Semaphore   mutex = 1;
#define PRODUCER    0
#define CONSUMER    1
 
//记录型信号量的wait操作
void waitS(int type, Semaphore *s)
{
    //等待不成功则输出信息，然后主动堵塞
    while (*s == 0)
    {
        if (type == PRODUCER)
        {
            printf("Producer is waiting for record semaphore......\n\n");
        }
        else if (type == CONSUMER)
        {
            printf("Consumer is waiting for record semaphore......\n\n");
        }
        //没有实现block原语，用Sleep函数吧
        Sleep(1);
    }
 
    //等待成功则执行这条语句
    (*s)--;
}
void waitM(int type, Semaphore *s)
{
    if (*s == 1)
    {
        (*s) = 0;
    }
    else
    {
        if (type == PRODUCER)
        {
            printf("Producer is waiting for mutex......\n\n");
        }
        else if (type == CONSUMER)
        {
            printf("Consumer is waiting for mutex......\n\n");
        }
        Sleep(1);
    }
}
void signalS(Semaphore *s)
{
    (*s)++;
}
 
//互斥型信号量的signal操作
void signalM(Semaphore *s)
{
    *s = 1;
}

int __stdcall producer(LPVOID lpThreadParameter)
{
    int nextp;
    while(1)
    {
        nextp = GetTickCount();
        printf("Produce an item.\n");
        waitS(PRODUCER,&empty);
        waitM(PRODUCER,&mutex);
        printf("Producer entered critical section.\n");
        buffer[in] = nextp;
        in = (++in) % BUFFER_SIZE;
        printf("After 小和尚 leaves critical section in = %d, out = %d, full = %d, empty = %d.\n",in,out,full+1,empty);
        signalM(&mutex);
        signalS(&full);
        printf("小和尚 left critical section.\n\n");
    }
    return 0;
}

int __stdcall consumer(LPVOID lpThreadParameter)
{
    int nextc;
 
    while(1)
    {
        waitS(CONSUMER,&full);
        waitM(CONSUMER,&mutex);
        printf("Consumer entered critical section.\n");
        nextc = buffer[out];
        out = (++out) % BUFFER_SIZE;
        printf("After 老和尚 leaves critical section in = %d, out = %d, full = %d, empty = %d.\n",in,out,full,empty+1);
        signalM(&mutex);
        signalS(&empty);
        printf("老和尚 left critical section.\n");
    }
    return 0;
}
 
 
 
int main()
{
    HANDLE  hProducer,hConsumer;
    //标准输出重定向到文件中，以方便查看运行过程
    freopen("D:\\stdout.txt","w",stdout);
    hProducer = CreateThread(NULL,0,producer,NULL,0,NULL);
    hConsumer = CreateThread(NULL,0,consumer,NULL,0,NULL);

    Sleep(500);
    TerminateThread(hProducer,0);
    TerminateThread(hConsumer,0);
    return 0;
}
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
