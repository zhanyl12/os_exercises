# lab2 SPOC思考题

NOTICE
- 有"w4l1"标记的题是助教要提交到学堂在线上的。
- 有"w4l1"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。

## 个人思考题
---

x86保护模式中权限管理无处不在，下面哪些时候要检查访问权限()  (w4l1)
- [x] 内存寻址过程中
- [x] 代码跳转过程中
- [x] 中断处理过程中
- [ ] ALU计算过程中
 
> 前三个需要。这里假定ALU完成计算所需数据都已经在CPU内部了。


请描述ucore OS建立页机制的准备工作包括哪些步骤？ (w4l1) 
```
  + 采分点：说明了ucore OS在让页机制正常工作的主要准备工作
  - 答案没有涉及如下3点；（0分）
  - 描述了对GDT的初始化,完成了段机制（1分）
  - 除第二点外进一步描述了对物理内存的探测和空闲物理内存的管理。（2分）
  - 除上述两点外，进一步描述了页表建立初始过程和设置CR0控寄存器某位来使能页（3分）

 ```
- [x]  

> 在ucore os的页机制的建立过程当中首先要处理完成段机制，完成对于GDT的初始化，然后进行线性地址的映射转换，建立页表项，建立页表，在保护模式下设置CR0寄存器bit 31（PG）进行使能页。 

---

## 小组思考题
---

（1）（spoc）请用lab1实验的基准代码（即没有修改的需要填空的源代码）来做如下实验： 执行`make qemu`，会得到一个输出结果，请给出合理的解释：为何qemu退出了？【提示】需要对qemu增加一些用于基于执行过的参数，重点是分析其执行的指令和产生的中断或异常。 

- [x]  

> finish lab1

（2）(spoc)假定你已经完成了lab1的实验,接下来是对lab1的中断处理的回顾：请把你的学号对37(十进制)取模，得到一个数x（x的范围是-1<x<37），然后在你的答案的基础上，修init.c中的kern_init函数，在大约36行处，即

```
    intr_enable();              // enable irq interrupt
```
语句之后，加入如下语句(把x替换为你学号 mod 37得的值)：
```
    asm volatile ("int $x");
```    
然后，请回答加入这条语句后，执行`make qemu`的输出结果与你没有加入这条语句后执行`make qemu`的输出结果的差异，并解释为什么有差异或没差异？ 

- [x]  

> 
> 有差异，我的学号是2012011388，余数是14，在加入这一行代码之后，实际上是人工产生了中断，qemu保存了中断trap现场，那个14相当于是一个中断号码，在输出的时候额外地输出了其他信息：
trapframe at 0x7b54
  edi  0x00000001
  esi  0x00000000
  ebp  0x00007bc8
  oesp 0x00007b74
  ebx  0x00010094
  edx  0x000000a1
  ecx  0x00000000
  eax  0x000000ff
  ds   0x----0010
  es   0x----0010
  fs   0x----0023
  gs   0x----0023
  trap 0x0000000d General Protection
  err  0x00001368
  eip  0x00102043
  cs   0x----0008
  flag 0x00000002 IOPL=0
kernel panic at kern/trap/trap.c:253:
    unexpected trap in kernel.
Welcome to the kernel debug monitor!!
Type 'help' for a list of commands.  
对应的中断名称是磁盘控制器中断--软磁盘


（3）对于lab2的输出信息，请说明数字的含义
```
e820map:
  memory: 0009fc00, [00000000, 0009fbff], type = 1.
  memory: 00000400, [0009fc00, 0009ffff], type = 2.
  memory: 00010000, [000f0000, 000fffff], type = 2.
  memory: 07ee0000, [00100000, 07fdffff], type = 1.
  memory: 00020000, [07fe0000, 07ffffff], type = 2.
  memory: 00040000, [fffc0000, ffffffff], type = 2.
```
修改lab2，让其显示` type="some string"` 让人能够读懂，而不是不好理解的数字1,2  (easy) 
- [x]  

> 在pmm.c当中的page_init函数中，我们可以知道这些信息其实是有这个函数负责输出的，这个type是map对应的type，在memlayout.h的文件当中的map的结构体当中我们找到map的type当中对应的，知道这个type究竟是否是有效的

（4）(spoc)有一台只有页机制的简化80386的32bit计算机，有地址范围位0~256MB的物理内存空间（physical memory），可表示大小为256MB，范围为0xC0000000~0xD0000000的虚拟地址空间（virtual address space）,页大小（page size）为4KB，采用二级页表，一个页目录项（page directory entry ，PDE）大小为4B,一个页表项（page-table entries PTEs）大小为4B，1个页目录表大小为4KB，1个页表大小为4KB。
```
PTE格式（32 bit） :
  PFN19 ... PFN0|NOUSE9 ... NOUSE0|WRITABLE|VALID
PDE格式（32 bit） :
  PT19 ... PT0|NOUSE9 ... NOUSE0|WRITABLE|VALID
 
其中：
NOUSE9 ... NOUSE0为保留位，要求固定为0
WRITABLE：1表示可写，0表示只读
VLAID：1表示有效，0表示无效
```

假设ucore OS已经为此机器设置好了针对如下虚拟地址<-->物理地址映射的二级页表，设置了页目录基址寄存器（page directory base register，PDBR）保存了页目录表的物理地址（按页对齐），其值为0。已经建立好了从0x1000~41000的二级页表，且页目录表的index为0~63的页目录项的(PT19 ... PT0)的值=(index+1)。
请写出一个translation程序（可基于python, ruby, C, C++，LISP等），输入是一个虚拟地址和一个物理地址，能够自动计算出对应的页目录项的index值,页目录项内容的值，页表项的index值，页表项内容的值。即(pde_idx, pde_ctx, pte_idx, pte_cxt)

请用如下值来验证你写的程序的正确性：
```
va 0xc2265b1f, pa 0x0d8f1b1f
va 0xcc386bbc, pa 0x0414cbbc
va 0xc7ed4d57, pa 0x07311d57
va 0xca6cecc0, pa 0x0c9e9cc0
va 0xc18072e8, pa 0x007412e8
va 0xcd5f4b3a, pa 0x06ec9b3a
va 0xcc324c99, pa 0x0008ac99
va 0xc7204e52, pa 0x0b8b6e52
va 0xc3a90293, pa 0x0f1fd293
va 0xce6c3f32, pa 0x007d4f32
```

参考的输出格式为：
```
va 0xcd82c07c, pa 0x0c20907c, pde_idx 0x00000336, pde_ctx  0x00037003, pte_idx 0x0000002c, pte_ctx  0x0000c20b
```

- [x]  

> #include <stdio.h>
#include <stdlib.h>
unsigned int  mem[128][32];
int char2int(char a){
	return (a>58)?(a-87):(a-48);
}
int value(char* a,int start){
	int i=0,sum=0;
	for(;i<8;i++){
		sum*=16;
		sum+=char2int(a[start+i]);
	}
	return sum;
}
void catch(int value){
	//printf("%d %d\n",value>>10,mem[17][value>>10]);
	if(mem[17][value>>10]/128==0){
		printf("	--> pde index:0x%x  pde contents:(valid 0, pfn 0x%x)\n",value>>10,mem[17][value>>10]%128);
      		printf("		--> Fault (page directory entry not valid)\n");
	}else{
		printf("	--> pde index:0x%x  pde contents:(valid 1, pfn 0x%x)\n",value>>10,mem[17][value>>10]%128);
		if(mem[mem[17][value>>10]%128][(value>>5)%32]/128==0){
			printf("		--> pte index:0x%x  pte contents:(valid 0, pfn 0x%x)\n",(value>>5)%32,mem[mem[17][value>>10]%128][(value>>5)%32]%128);
			printf("			--> Fault (page table entry not valid)\n");
		}else{
			printf("		--> pte index:0x%x  pte contents:(valid 1, pfn 0x%x)\n",(value>>5)%32,mem[mem[17][value>>10]%128][(value>>5)%32]%128);
			int address=value%32+(mem[mem[17][value>>10]%128][(value>>5)%32]%128)*32;
			//printf("%d\n",address);
			printf("			 --> Translates to Physical Address 0x%x --> Value: 0x%x\n",address,mem[address/32][address%32]);
		}
	}
}
int main(){
	FILE *fp=fopen("data.txt","rt");
	int index=0;
	char a[1024];
	int offset = 9;
	unsigned int page=0x00;
	int data1=0,data2=0;
	int pde_idx=0,pde_ctx,pte_idx,pte_ctx;
	while(!feof(fp)){
		fgets(a,1000,fp);
		for(;i<32;i++){
			//mem[index][i]=value2(a,offset+3*i);
			data1=value(a,5);
			data2=value(a,20);
			printf("%d %d \n",data1,data2);	
			pde_idx=data1>>22;
			pde_ctx=((data1>>22-0x300)+1)<<12+3;
			pte_idx=(data1>>22-0x300+1);
			pte_ctx=(data2>>12)
		}
		index++;
	};
	return 0;
}


---

## 开放思考题

---

（1）请简要分析Intel的x64 64bit体系结构下的分页机制是如何实现的 
```
  + 采分点：说明Intel x64架构的分页机制的大致特点和页表执行过程
  - 答案没有涉及如下3点；（0分）
  - 正确描述了x64支持的物理内存大小限制（1分）
  - 正确描述了x64下的多级页表的级数和多级页表的结构（2分）
  - 除上述两点外，进一步描述了在多级页表下的虚拟地址-->物理地址的映射过程（3分）
 ```
- [x]  

>  x64 64bit在网上查明可以最多可以支持物理内存大小为64G，x64下多级页表的级数一般为三级，有PDP,PDE,PTE然后再结合offset是可以构成一个页表项，按照约定由若干个页表项组成一个多级页表，我们拿到一个虚拟地址按照之前的约定，将这个虚拟地址按位进行划分，事实上这些虚拟地址划分出来的部分都会当作偏移量进入到运算当中，首先我们找到PDP基址，加上对应偏移量得到PDE地址的存储基址，随后继续不断下去……最终得到了实际的物理地址，完成了映射过程。

（2）Intel8086不支持页机制，但有hacker设计过包含未做任何改动的8086CPU的分页系统。猜想一下，hacker是如何做到这一点的？提示：想想MMU的逻辑位置

- [x]  

> hacker是通过在CPU和内存之间做一个虚拟的分页调度单元MMU，使得intel8086看起来似乎是支持分页机制。

---
