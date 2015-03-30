#lec9 虚存置换算法spoc练习

## 个人思考题
1. 置换算法的功能？

2. 全局和局部置换算法的不同？

3. 最优算法、先进先出算法和LRU算法的思路？

4. 时钟置换算法的思路？

5. LFU算法的思路？

6. 什么是Belady现象？

7. 几种局部置换算法的相关性：什么地方是相似的？什么地方是不同的？为什么有这种相似或不同？

8. 什么是工作集？

9. 什么是常驻集？

10. 工作集算法的思路？

11. 缺页率算法的思路？

12. 什么是虚拟内存管理的抖动现象？

13. 操作系统负载控制的最佳状态是什么状态？

## 小组思考题目

----
(1)（spoc）请证明为何LRU算法不会出现belady现象


(2)（spoc）根据你的`学号 mod 4`的结果值，确定选择四种替换算法（0：LRU置换算法，1:改进的clock 页置换算法，2：工作集页置换算法，3：缺页率置换算法）中的一种来设计一个应用程序（可基于python, ruby, C, C++，LISP等）模拟实现，并给出测试。请参考如python代码或独自实现。
 - [页置换算法实现的参考实例](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab3/page-replacement-policy.py)
 - 
 ```
 #include<iostream>

using namespace std;
int window_size=5;
int my_time=0;
int current[6];
int last_visit[11];

void show_result()
{
	cout<<"current page is :"<<endl;
	for(int i=1;i<=window_size;i++)
	{
		cout<<current[i]<<" ";
	}
	cout<<endl;
	cout<<"last visit is:"<<endl;
	for(int i=1;i<=10;i++)
	{
		cout<<i<<" "<<last_visit[i]<<endl;
	}
}

void change_visit(int page)
{
	last_visit[page]=my_time;
}

void change_current(int page)
{
	int temp=last_visit[current[1]];
	int answer=1;
	for(int i=1;i<=window_size;i++)
	{
		if(temp>last_visit[current[i]])
		{
			temp=last_visit[current[i]];
			answer=i;
		}
	}
	current[answer]=page;
}

int main()
{
	for(int i=1;i<=10;i++)
	{
		last_visit[i]=0;
	}
	last_visit[4]=-1;
	last_visit[2]=last_visit[3]=-2;
	for(int i=1;i<=window_size;i++)
	{
		current[i]=i;
	}//完成初始化，给定一定的测试用例 
	int page;
	while(1)
	{
		my_time++;
		cout<<"please input the page_number you want to visit(from 1 to 10)"<<endl;
		cin>>page;
		if(page==-1)
		{
			break;
		}
		bool found=false;
		for(int i=1;i<=5;i++)
		{
			if(current[i]==page)
			{
				found=true;
			}
		}
		if(found==true)
		{
			change_visit(page);
		}
		else if(found==false)
		{
			change_current(page);
			change_visit(page);
		}
		show_result();
	}
	system("pause");
	return 0;
}

 ```
 
## 扩展思考题
（1）了解LIRS页置换算法的设计思路，尝试用高级语言实现其基本思路。此算法是江松博士（导师：张晓东博士）设计完成的，非常不错！

参考信息：

 - [LIRS conf paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang02_LIRS.pdf)
 - [LIRS journal paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang05_LIRS.pdf)
 - [LIRS-replacement ppt1](http://dragonstar.ict.ac.cn/course_09/XD_Zhang/(6)-LIRS-replacement.pdf)
 - [LIRS-replacement ppt2](http://www.ece.eng.wayne.edu/~sjiang/Projects/LIRS/sig02.ppt)
