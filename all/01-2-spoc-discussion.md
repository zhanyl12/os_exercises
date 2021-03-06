# 操作系统概述思考题

## 个人思考题

---

分析你所认识的操作系统（Windows、Linux、FreeBSD、Android、iOS）所具有的独特和共性的功能？
- [x]  

>  
操作系统的共性功能是微处理器的管理功能，内存管理功能，外部设备管理功能，文件
管理功能和进程管理功能，这五个功能简称为操作系统的五大功能，可以认为是操作系统的
共性。
windows：用户界面比较友好，上手比较快，但是不开源，收费……
linux：模块化比较好，源代码开源，使用命令行操作，安全可靠性号
freeBSD：高性能，高可靠性，是真正的32位操作系统，任务分配高效
android：不受开放商限制，开放性非常高，拥有很多的开发者
ios：界面华丽，应用很多，数据安全性强
参考资料：wenku.baidu.com/link?url=TQYCt8-r81NppULlIwc0Mtl42uXhN80DwP9qtv1-

pXVr9PKRPi7UeGyFXUPQ_SndONJI7G4frUl12__XqnbB3dQD_2RqLdgCaZxEYAvDJse
http://www.cnblogs.com/sunsonbaby/archive/2004/08/26/36671.html
http://www.apk3.com/androidnews/html/1184.html
http://wenku.baidu.com/link?

url=QFxnQRjncUpFe5Pbki14FsF44520l7YJlbUDK6tm50i4fyPg5P5AxXD30ByJ9CXUyNq8iBrKW5Pt

7L8oFkA7m8X4U5MIoRq95T6NHcb4_gq

请总结你认为操作系统应该具有的特征有什么？并对其特征进行简要阐述。
- [x]  

>1、界面友好，操作简单：能够让用户准确地进行操作，零基础用户可以比较快地上手，能
让用户很容易明白这些交互按钮图标是干什么的。
2、重视安全性，注意保护隐私：用户系统的隐私数据不能外泄，某一个用户的信息不能泄
露给其他的用户。
3、鲁棒稳定：稳定不容易崩溃，比较耐用，不容易出现其他的问题
4、支持的种类多：能够支持多种软硬件和外设，给操作系统的使用者一个好的体验
5、开源免费：开源可以给这个操作系统提供更多的上升空间，另外……免费，对   
看了课件发现应当为以下四个特征：
并发：计算机中存在多个运行程序，需要操作系统管理调度
虚拟：多道程序技术，使每一个用户都感觉有一个计算机为他服务
共享：同时访问，互斥共享
异步：程序执行不是一次到底而是走走停停，只要运行环境相同，操作系统保证程序运行结果也相同。


请给出你觉得的更准确的操作系统的定义？
- [x]  

> 操作系统（英语；Operating System，简称OS）是一管理电脑硬件与软件资源的程序，同时也是计算机系统的内核与基石。操作系统身负诸如管理与配置内存、决定系统资源供需的优先次序、控制输入与输出设备、操作网络与管理文件系统等基本事务。操作系统是管理计算机系统的全部硬件资源包括软件资源及数据资源;控制程序运行;改善人机界面;为其它应用软件提供支持等，使计算机系统所有资源最大限度地发挥作用，为用户提供方便的、有效的、友善的服务界面，操作系统是一个庞大的管理控制程序。
参考资料：http://zhidao.baidu.com/question/43422198.html  

你希望从操作系统课学到什么知识？
- [x]  

> 我希望可以从操作系统课程当中更深入地了解整个计算机的构造，学习一些操作系统软硬件结合的相关知识，了解计算机内部运行的机理，培养一个可以写操作系统的能力。  

---

## 小组讨论题

---

目前的台式PC机标准配置和价格？
- [x]  

> 4核CPU,1G独显，500G硬盘，4G内存，根据声卡，USB接口，网卡集成等不同价格有所不同，
参考了几个网站，如果自己装配电脑价格比较便宜，在3000元左右，而如果购买品牌电脑折
扣价也会在45千元左右。

你理解的命令行接口和GUI接口具有哪些共性和不同的特征？
- [x]  

> 共性：两者均是实际上执行了程序代码，都是执行了代码里面构造好的函数，都是属于人机
交互的一个接口或者说是途径，都是方便了用户的操作。
特性：命令行的运行速度更快一些，显得非常专业，逼格高，适合专业人士或者是使用的较
为熟练的人员使用；而图形界面速度可能相对慢一点，但是它交互性好，界面友好，让人看
上去感觉很舒服而且新手容易上手，给很多人提供了较大的方便，普及了技术使用。

为什么现在的操作系统基本上用C语言来实现？
- [x]  

>c语言紧凑简洁，灵活方便，运算符和数据结构都非常方便，它的语法限制并不严格，给程
序很大的设计自由度，允许直接访问物理地址，c语言生成代码质量高，程序执行效率高，
适用范围大，可移植性好。由于它本身具有很多的优点，越来越多的系统软件开始使用它，
为了兼容性，形成了一个滚雪球效应，所以现在的操作系统基本上用c语言实现。  

为什么没有人用python，java来实现操作系统？
- [x]  

> c语言是将代码编译为机器语言而python和java并不能像c语言一样直接编译为机器语言，它
是依赖于自己的一套编译方法，因而虽然有可能使用python和java来实现操作系统，但是使
用起来非常麻烦，并不能减轻负担。

请评价用C++来实现操作系统的利弊？
- [x]  

> c++一般实现操作系统是使用的面向对象，对于编写操作系统这样的大型工程来说编写难度
可能会小一些，但是代码的提及会非常大，冗余代码非常多，速度比较慢，相对于c语言的
面向过程，运行速度快，程序代码体积小，c++没有什么优势，感觉c语言更加适合操作系统
的编写。
参考资料：http://zhidao.baidu.com/link?url=xCg0zJCvaH_KzAwg7NnxI4JWu-
WL5lPmdcN7Ut25srkIiVf46ECNHdsDyy4ggNkOGz8TqWPR0QhfNllLG7GT6K 

---

## 开放思考题

---

请评价微内核、单体内核、外核（exo-kernel）架构的操作系统的利弊？
- [x]  

>  微内核：本身只提供最基本的操作系统的功能，比如进程调度与消息传递等，其他的功能由其独立的模块提供，每个独立的功能模块都可以是一个进程，模块化设计操作系统，设计者只需要关注自己的模块，可以动态更新其他模块，可以动态使能或者禁止以释放计算机资源，但是性能上较慢。
单体内核：速度快，但自主性差，留给自己使用的资源比较少。
外核：操作效率高，但是提供的服务有限。
参考资料：http://baike.baidu.com/link?url=nEhFVEkwML1LN8DCU3w8hrYTvTqpfvepgMFm2nJktCc3jNVwzNWbs5ULox9fywJFBd0wfaxgCOZhy9JfStB9dK
http://blog.csdn.net/stephanxu/article/details/5385772

请评价用LISP,OCcaml, GO, D，RUST等实现操作系统的利弊？
- [x]  

>  这几种语言还没有听说过……pass

进程切换的可能实现思路？
- [x]  

> 首先由计算机对于各个任务的总量有一个总体的评估，将时钟分成若干份，根据任务的总量进行任务的切换，让人感受到任务是在同时并行的。 

计算机与终端间通过串口通信的可能实现思路？
- [x]  

> 类似于计原大作业的做法……当串口有收发的时候就停掉时钟进行串口的通信然后再进行计算机本身的操作计算。 

为什么微软的Windows没有在手机终端领域取得领先地位？
- [x]  

> 我认为微软windows不开源而且对于其他公司做的软件兼容性稍差，而且是收费的，这导致它在手机终端领域不能让众多功能百花齐放，微软公司并不能像苹果一样主攻手机市场，它的单一功能和界面并非是那样美观和苹果公司诸多高端软件相比在竞争中难以取得优势。 

你认为未来（10年内）的操作系统应该具有什么样的特征和功能？
- [x]  

> 未来的操作系统应当支持开源，不收费，在隐私保护方面做得更加完美，兼容各种软件，系统界面更加友善，高效，让小白容易上手。  

---
