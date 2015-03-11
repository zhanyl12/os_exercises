# lab1 SPOC思考题

## 个人思考题

NOTICE
- 有"w2l2"标记的题是助教要提交到学堂在线上的。
- 有"w2l2"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。
---

请描述ucore OS配置和驱动外设时钟的准备工作包括哪些步骤？ (w2l2)
```
  + 采分点：说明了ucore OS在让外设时钟正常工作的主要准备工作
  - 答案没有涉及如下3点；（0分）
  - 描述了对IDT的初始化，包了针时钟中断的中断描述符的设置（1分）
  - 除第二点外，进一步描述了对8259中断控制器的初始过程（2分）
  - 除上述两点外，进一步描述了对8253时钟外设的初始化，或描述了对EFLAG操作使能中断（3分）
 ```
- [x]  

>  外设时钟的设置和驱动工作的步骤主要是ucore系统配置和驱动外设的一般工作，另外需要注意特别的中断外设序号即可，具体步骤如下：
首先对于中断描述符表进行相应的初始化，注意要设置好描述符表中对应于时钟中断的项。
然后要配置好8259中断控制器，相应的外部设备就能够产生中断。
然而时钟外设是属于特定的外设，除了以上的步骤还需要完成特定外设对应的初始化，设置时钟外设8253，然后再设置STI使能中断，然后外设时钟就可以正常工作了。

lab1中完成了对哪些外设的访问？ (w2l2)
 ```
  + 采分点：说明了ucore OS访问的外设
  - 答案没有涉及如下3点；（0分）
  - 说明了时钟（1分）
  - 除第二点外，进一步说明了串口（2分）
  - 除上述两点外，进一步说明了并口，或说明了CGA，或说明了键盘（3分）
 ```
- [x]  

>  lab1中对于外设的访问我们可以结合课件当中的内容去看，首先在输出tick当中我们使用到了时钟外设，串口外设，并口外设，以及CGA也就是显示器外设，另外我们在整个lab当中其实还用到了键盘外设。

lab1中的cprintf函数最终通过哪些外设完成了对字符串的输出？ (w2l2)
 ```
  + 采分点：说明了cprintf函数用到的3个外设
  - 答案没有涉及如下3点；（0分）
  - 说明了串口（1分）
  - 除第二点外，进一步说明了并口（2分）
  - 除上述两点外，进一步说明了CGA（3分）
 ```
- [x]  

>  cprintf函数通过串口外设和并口外设进行信息的交互，最终通过CGA显示器外设把结果显示出去，综上所述，我们在cprintf函数的时候使用了串口外设和并口外设以及CGA外设。

---

## 小组思考题

---

lab1中printfmt函数用到了可变参，请参考写一个小的linux应用程序，完成实现定义和调用一个可变参数的函数。(spoc)
- [x]  

> \#include \<stdio.h\><br />
\#include \<stdarg.h\><br />
\#include \<unistd.h\><br />
int addall(int num,...){<br />
	va_list ap;<br />
	int sum=0;<br />
	va_start(ap, num);<br />
	while(num--){<br />
    	sum+=va_arg(ap,int);<br />
	}<br />
  va_end(ap);<br />
	return sum;<br />
}<br />
int main(){<br />
	printf("%d\n",addall(3,20,1,2));<br />
}<br />
* va_list 是一个字符指针，可以理解为指向当前参数的一个指针，取参必须通过这个指针进行
* va_start使得ap指向不确定参数前的那个值，在这份代码中，是总数num，这是对ap 的初始化，
* va_arg用来依次获得指定类型的参数
* va_end用来回收这个指针，以免发生安全上的隐患。<br />
以上的传参方法虽然很好用，但是也有两方面的问题：
* 输入参数的类型随意，这使得参数很容易以一个不正确的类型获取一个值(譬如输入一个float，却以int型去获取)，这样做不好调试
* 变参表的大小并不能在运行时获取（每次传参时同时告诉参数的数量也不现实），很容易访问越界


如果让你来一个阶段一个阶段地从零开始完整实现lab1（不是现在的填空考方式），你的实现步骤是什么？（比如先实现一个可显示字符串的bootloader（描述一下要实现的关键步骤和需要注意的事项），再实现一个可加载ELF格式文件的bootloader（再描述一下进一步要实现的关键步骤和需要注意的事项）...） (spoc)
- [x]  

> 从零实现lab1首先我们要实现一个可以显示字符串的bootloader，这个bootloader可以初始化寄存器当中的内容，可以完成实模式到保护模式的转化，可以与串口，并口以及CGA显示器通信达到显示字符串的目的；随后我们实现一个可以加载ELF格式的bootloader，这个bootloader可以加载elf文件，需要注意的是这两个bootloader都应该满足小于512个字节的要求；接着我们实现可以显示函数调用关系，能够模拟一个函数栈的ucore，它可以比较清楚地显示函数的调用关系；最后我们再实现一个可以解决处理中断的ucore，注意要设置中断描述符表能够初始化，并且涵盖各种中断的情况而且有相应的中断服务例程。


如何能获取一个系统调用的调用次数信息？如何可以获取所有系统调用的调用次数信息？请简要说明可能的思路。(spoc)
- [x]  

> 我们在lab8的answer里面的kern当中的syscall.c文件可以看到许多关于系统调用的函数，这里面有几乎在lab当中可能遇到的系统调用函数，我们可以在整个的操作系统软件当中加入这样的syscall的一个编码文件，对于系统调用的调用次数信息我们可以在这个syscall文件当中加入统计数字的变量，在每一个类似于static int
sys_fstat(uint32_t arg[]) {
    int fd = (int)arg[0];
    struct stat *stat = (struct stat *)arg[1];
    return sysfile_fstat(fd, stat);
}的函数当中，我们加入对于数字统计的变化，比如sys_sum统计数字，我们一开始将它初始化为0，在每一个系统调用函数中我们对于这个变量加一，最终得到系统调用的次数，如果希望得到一些特殊系统调用的次数信息，我们也可以单独进行统计，如果需要输出这个信息就可以输出它。当然我们也可以使用strace工具，它也可以获取系统调用的信息。

如何裁减lab1, 实现一个可显示字符串"THU LAB1"且依然能够正确加载ucore OS的bootloader？如果不能完成实现，请说明理由。
- [x]  

> 经过和老师的讨论，得出以下结论：bootloader确实是可以控制外设并且可以识别elf文件，但是我们需要注意bootloader只有512个字节，这512个字节如果按照要求实现一个可以显示字符串“THU LAB1”而且可以正确加载ucore的bootloader将是极其困难几乎不可能的，稍有不慎就会出现超字节或者是功能无法实现全的情况，因而我们认为还不能实现。

对于ucore_lab中的labcodes/lab1,我们知道如果在qemu中执行，可能会出现各种稀奇古怪的问题，比如reboot，死机，黑屏等等。请通过qem的分析功能来动态分析并回答lab1是如何执行并最终为什么会出现这种情况？
- [x]  

> 本题和下一题并不太清楚正确答案，这是按照自己的理解写一下想法，使用qemu可以大致看一看代码的执行过程，当出现死机，蓝屏等问题的时候，可以知道程序也就是不运行了，我们跟踪运行的过程可以看看每个寄存器以及函数调用栈当中的内容，结合代码执行到的位置可以看看lab1是为什么出现了这种情况。

对于ucore_lab中的labcodes/lab1,如果出现了reboot，死机，黑屏等现象，请思考设计有效的调试方法来分析常在现背后的原因。
- [x]  

> 使用qemu的分析功能进行，另外使用strace我们可以看到究竟是哪一个系统调用有了error出现从而找出问题出现在哪。

---

## 开放思考题

---

如何修改lab1, 实现在出现除零错误异常时显示一个字符串的异常服务例程的lab1？
- [x]  

> 我们在lab1的error.h当中可以找到各种类型的错误，我们在error.h的文件当中声明一个新的类型错误，用于单独处理除零错误异常，随后我们可以在trap当中去处理这个问题，给出一个除零错的相应输出显示信息，如果除法运算的除数为0，那么我们执行这个软中断，在异常时就会显示一个字符串作为异常服务例程。


在lab1/bin目录下，通过`objcopy -O binary kernel kernel.bin`可以把elf格式的ucore kernel转变成体积更小巧的binary格式的ucore kernel。为此，需要如何修改lab1的bootloader, 能够实现正确加载binar格式的ucore OS？ (hard)
- [x]  

>

GRUB是一个通用的bootloader，被用于加载多种操作系统。如果放弃lab1的bootloader，采用GRU来加载ucore OS，请问需要如何修改lab1, 能够实现此需求？ (hard)
- [x]  

>


如果没有中断，操作系统设计会有哪些问题或困难？在这种情况下，能否完成对外设驱动和对进程的切换等操作系统核心功能？
- [x]  

>  如果没有中断，操作系统在响应外设以及进行进程切换的时候就会非常缓慢，比如在计原作业当中如果串口当中有输入的话，我们没有中断流水线就直接进行串口外设等操作，这样会导致整个计算机会降慢本身的速度而去适应外设的速度，我们知道外设的速度是缓慢的，虽然没有中断的情况下我们还是可以完成对于外设驱动和对于进程的切换等操作系统的核心功能，但是速度会慢的令人发指，因而我们必须要采用中断的方法对于操作系统进行设计。

---