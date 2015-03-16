# lec5 SPOC思考题


NOTICE
- 有"w3l1"标记的题是助教要提交到学堂在线上的。
- 有"w3l1"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。


## 个人思考题
---

请简要分析最优匹配，最差匹配，最先匹配，buddy systemm分配算法的优势和劣势，并尝试提出一种更有效的连续内存分配算法 (w3l1)
```
  + 采分点：说明四种算法的优点和缺点
  - 答案没有涉及如下3点；（0分）
  - 正确描述了二种分配算法的优势和劣势（1分）
  - 正确描述了四种分配算法的优势和劣势（2分）
  - 除上述两点外，进一步描述了一种更有效的分配算法（3分）
 ```
- [x]  

>  最先匹配优点是简单，高地址空间会有大块的空闲区域可以处理需要内存比较大的应用，而它的缺点是会产生外部碎片，在分配大块内存的时候速度慢；最优匹配优点是尺寸小的时候效果好，能够避免大的分区被拆，减小外部碎片大小，相对简单，缺点是会产生外部碎片，释放分区慢，容易产生无用的小碎片；最差匹配优点是当中等大小分配比较多的时候效果好，能够避免出现太多的小碎片，缺点是释放分区慢，会产生外部碎片，难以分配大的分区；buddy伙伴系统的内存分配方法的优点是地址非常整齐，访问起来迅速而且不会产生不能利用的小碎片，但是它的缺点是可能空间利用率很低，合并起来速度有些慢。我认为可以使用人工智能的方法将最优匹配，最差匹配和最先匹配结合起来，当一个新的进程来到时，计算机系统智能评估，根据它的需要的各种分区的数量决定采取哪一种匹配，这种方法虽然消耗了一定的计算时间，但是可以做到基本是完美地利用了内存空间。

## 小组思考题

请参考ucore lab2代码，采用`struct pmm_manager` 根据你的`学号 mod 4`的结果值，选择四种（0:最优匹配，1:最差匹配，2:最先匹配，3:buddy systemm）分配算法中的一种或多种，在应用程序层面(可以 用python,ruby,C++，C，LISP等高语言)来实现，给出你的设思路，并给出测试用例。 (spoc)

--- 
> 学号2012011388 实现最优匹配算法，测试用例已经在代码中写入，使用c++语言进行编写。
#include<iostream>  
#include<stdio.h>  
#include<string.h>  
#include<cstring>  

using namespace std;  
struct empty  
{  
	int start;  
	int size;  
	int end;  
}my_free[10000],temp;  
  
struct busy  
{  
	int start;  
	int size;  
	int end;  
	string s;//记录进程名称   
}full[10000];  
int free_number=1;  
int full_number=0;  

void init()   
{  
	my_free[1].start=1;  
	my_free[1].size=10000;  
	my_free[1].end=10000;  
}//初始化内存  

void sort_free1(int number)  
{  
	for(int i=1;i<=free_number;i++)  
	{  
		if(my_free[i].size>my_free[free_number].size)  
		{  
			temp=my_free[free_number];  
			for(int k=i;k<=free_number-1;k++)  
			{  
				my_free[k+1]=my_free[k];  
			}  
			my_free[i]=temp;  
			break;  
		}  
	}  
}  

void malloc(string name,int malloc_size)  
{  
	if(malloc_size%4!=0)  
	{  
		for(int i=1;i<=3;i++)  
		{  
			if((malloc_size+i)%4==0)  
			{  
				malloc_size=malloc_size+i;  
				break;  
			}//处理地址对齐,4   
		}  
	}  
	cout<<malloc_size<<endl;  
	bool success=false;  
	for(int i=1;i<=free_number;i++)  
	{  
		if(my_free[i].size>=malloc_size)  
		{  
			success=true;//成功分配  
			my_free[i].size=my_free[i].size-malloc_size;  
			full_number++;  
			full[full_number].s=name;  
			full[full_number].start=my_free[i].start;  
			full[full_number].size=malloc_size;  
			full[full_number].end=full[full_number].start+malloc_size-1;  
			my_free[i].start=my_free[i].start+malloc_size;  
			sort_free1(i);  
			break;   
		}  
	}  
	for(int i=1;i<=free_number;i++)  
	{  
		cout<<my_free[free_number].start<<" "<<my_free[free_number].size<<endl;  
	}  
	cout<<success<<endl;  
	for(int i=1;i<=full_number;i++)  
	{  
		cout<<full[i].s<<" "<<full[i].size<<endl;  
	}  
	if(success==false)  
	{  
		cout<<"地址空间不足"<<endl;   
	}  
}   

void hebing()  
{  
	for(int i=1;i<=free_number;i++)  
	{  
		cout<<my_free[i].start<<" "<<my_free[i].size<<" "<<my_free[i].end<<endl;  
	}  
	bool change=true;  
	while(change==true)  
	{  
		change=false;  
		for(int i=1;i<free_number;i++)  
		{  
			if((my_free[free_number].start==my_free[i].end+1)||(my_free[free_number].end+1==my_free[i].start))  
			{  
				change=true;  
				temp=my_free[i];  
				for(int k=i;k<free_number;k++)  
				{  
					my_free[k]=my_free[k+1];  
				}  
				free_number--;  
				if(my_free[free_number].start==temp.end)  
				{  
					my_free[free_number].start=temp.start;  
					my_free[free_number].size=my_free[free_number].size+temp.size;  
				}  
				else if(my_free[free_number].end==temp.start)  
				{  
					my_free[free_number].end=temp.end;  
					my_free[free_number].size=my_free[free_number].size+temp.size;  
				}  
			}  
		}  
	}  
}//处理free空闲区域合并的情况   

void shifang(string name)  
{  
	bool success=false;  
	for(int i=1;i<=full_number;i++)  
	{  
		if(name==full[i].s)  
		{  
			success=true;  
			free_number++;  
			my_free[free_number].start=full[i].start;  
			my_free[free_number].size=full[i].size;  
			my_free[free_number].end=my_free[free_number].start+my_free[free_number].size-1;  
			cout<<my_free[free_number].start<<endl;  
			full_number--;  
			hebing();  
			sort_free1(free_number);  
			break;  
		}  
	}  
	if(success=false)  
	{  
		cout<<"找不到您想要关闭的进程"<<endl;  
	}  
}  

int main()  
{  
	init();  
	malloc("miao",22);  
	malloc("wang",15);  
	malloc("haha",34);  
	malloc("hehe",100);  
	shifang("wang");  
	//cout<<free_number<<endl;  
	shifang("haha");  
	malloc("caca",200);  
	malloc("e",16);  
	//cout<<free_number<<endl;  
	for(int i=1;i<=free_number;i++)  
	{  
		cout<<my_free[i].start<<" "<<my_free[i].size<<" "<<my_free[i].end<<endl;  
	}  
	system("pause");  
	return 0;  
}  


## 扩展思考题

阅读[slab分配算法](http://en.wikipedia.org/wiki/Slab_allocation)，尝试在应用程序中实现slab分配算法，给出设计方案和测试用例。


