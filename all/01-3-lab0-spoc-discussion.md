# lab0 SPOC思考题

## 个人思考题

---

能否读懂ucore中的AT&T格式的X86-32汇编语言？请列出你不理解的汇编语言。
- [x]  

>  http://www.imada.sdu.dk/Courses/DM18/Litteratur/IntelnATT.htm
有学过汇编语言，但是对于ucore当中的汇编语言只能读懂具有标准格式的东西，比如像是
movw %ax, %ds，orl $CR0_PE_ON, %eax等类似的代码，而对于一些没有太接触过的代码就
不太能理解，比如SEG_ASM(STA_X|STA_R, 0x0, 0xffffffff) ，.long gdt 等

虽然学过计算机原理和x86汇编（根据THU-CS的课程设置），但对ucore中涉及的哪些硬件设计或功能细节不够了解？
- [x]  

>  虽然说是学过计原和汇编但是这两门课多是以记忆为主，比如处理io有几种方法，我们只是记忆了下来，一张ppt讲解几种方法显然是浅尝辄止，并不能称之为了解，希望操作系统课程能够帮我们细致理解，比如io，中断，异常处理，文件管理等方面的知识。 


哪些困难（请分优先级）会阻碍你自主完成lab实验？
- [x]  

>  1、不能完全读懂源代码
   2、不知道能否使用所学的知识，通过编写代码把自己所学的知识表达出来

如何把一个在gdb中或执行过程中出现的物理/线性地址与你写的代码源码位置对应起来？
- [x]  

> 如果段机制启动而页机制没有启动的话，物理地址和线性地址是相等的
  如果段机制和页机制都启动的话，线性地址通过页机制处理，变成物理地址

了解函数调用栈对lab实验有何帮助？
- [x]  

>  了解函数调用栈，我们可以比较清晰地了解lab实验的运行机理，在读代码的时候可以比较容易地跟踪代码的执行过程从而更容易理解代码，做实验会更加容易。 

你希望从lab中学到什么知识？
- [x]  

>  1、能够比较容易理解题目当中所给的代码，提升自己在未来理解操作系统代码的能力
   2、能比较细致地理解io中断异常处理以及文件管理内存管理等知识，而不是像之前只是停留在记忆的阶段

---

## 小组讨论题

---

搭建好实验环境，请描述碰到的困难和解决的过程。
- [x]  

> 使用vmare虚拟机安装对应的gcc等，除了diff要根据提示安装diffutils之外没有遇到什么困难。

熟悉基本的git命令行操作命令，从github上的[ucore git repo](http://www.github.com/chyyuu/ucore_lab)下载ucore lab实验
- [x]  

> 已经下载。

尝试用qemu+gdb（or ECLIPSE-CDT）调试lab1
- [x]  

> 已经尝试。

对于如下的代码段，请说明”：“后面的数字是什么含义
```
/* Gate descriptors for interrupts and traps */
struct gatedesc {
    unsigned gd_off_15_0 : 16;        // low 16 bits of offset in segment
    unsigned gd_ss : 16;            // segment selector
    unsigned gd_args : 5;            // # args, 0 for interrupt/trap gates
    unsigned gd_rsv1 : 3;            // reserved(should be zero I guess)
    unsigned gd_type : 4;            // type(STS_{TG,IG32,TG32})
    unsigned gd_s : 1;                // must be 0 (system)
    unsigned gd_dpl : 2;            // descriptor(meaning new) privilege level
    unsigned gd_p : 1;                // Present
    unsigned gd_off_31_16 : 16;        // high bits of offset in segment
};
```
- [x]  

> 后面的数字代表了这个变量的二进制位数

对于如下的代码段，
```
#define SETGATE(gate, istrap, sel, off, dpl) {            \
    (gate).gd_off_15_0 = (uint32_t)(off) & 0xffff;        \
    (gate).gd_ss = (sel);                                \
    (gate).gd_args = 0;                                    \
    (gate).gd_rsv1 = 0;                                    \
    (gate).gd_type = (istrap) ? STS_TG32 : STS_IG32;    \
    (gate).gd_s = 0;                                    \
    (gate).gd_dpl = (dpl);                                \
    (gate).gd_p = 1;                                    \
    (gate).gd_off_31_16 = (uint32_t)(off) >> 16;        \
}
```
如果在其他代码段中有如下语句，
```
unsigned intr;
intr=8;
SETGATE(intr, 0,1,2,3);
```
请问执行上述指令后， intr的值是多少？
- [x]  

> 65538
首先在mmh文件当中include头文件stdint.h，随后使用c语言编写代码
#include "mmu.h"
#include<stdlib.h>
#include<stdio.h>
int main()
{
	struct gatedesc intr;
	intr.gd_off_15_0=0x0008;
	intr.gd_off_31_16=0x0000;
	SETGATE(intr, 0,1,2,3);
	printf("%ld",*((long*)&intr));
	return 0;
}
注意把后16位置为8，前面置为0，直接进行操作。

请分析 [list.h](https://github.com/chyyuu/ucore_lab/blob/master/labcodes/lab2/libs/list.h)内容中大致的含义，并能include这个文件，利用其结构和功能编写一个数据结构链表操作的小C程序
- [x]  

> 可以看出这个List.h的文件中实质上是定义了一个链表结构，这个结构体里面定义了指针头和尾部，在结构体的基础上定义了各种基于这个结构体的操作，比如有：init构建初始化，add，add_before,add_after增加元素，del删除元素，prev，next返回上一个和下一个元素，empty清空操作等。
小c程序请见丁延卓和陈刚同学answer，在课下我们进行了分工，我负责上一题目。

---

## 开放思考题

---

是否愿意挑战大实验（大实验内容来源于你的想法或老师列好的题目，需要与老师协商确定，需完成基本lab，但可不参加闭卷考试），如果有，可直接给老师email或课后面谈。
- [x]  

>  先做基本实验吧……

---

