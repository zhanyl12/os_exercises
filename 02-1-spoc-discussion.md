#lec 3 SPOC Discussion

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
 1. 比较UEFI和BIOS的区别。
 > BIOS是固化到计算机主板上面的程序，UEFI是用模块化、C语言风格的参数堆栈传递方式、动态链接的形式构建系统，它比BIOS更易于实现，容错和纠错特性也更强，从而缩短了系统研发的时间。更加重要的是，它运行于32位或64位模式，突破了传统16位代码的寻址能力，达到处理器的最大寻址，此举克服了BIOS代码运行缓慢的弊端，应该来说UEFI是一种更为先进的特殊的BIOS。

 1. 描述PXE的大致启动流程。
> 在获得控制权之前首先进行自我测试，随后取得IP地址，如果服务器相应要求就要回应IP地址和网关等信息，或者如果忽视了就会不进行相应，Bootprom 由 TFTP 通讯协议从服务器下载开机映像文件。，随后个人电脑通过这个开机映像文件开机，这个开机文件可以只是单纯的开机程式也可以是操作系统，开机映像文件将包含 kernel loader 及压缩过的 kernel，此 kernel 将支持NTFS root
系统，最后远程客户端根据下载的文件启动机器。

## 3.2 系统启动流程
 1. 了解NTLDR的启动流程。
 1. 了解GRUB的启动流程。
 1. 比较NTLDR和GRUB的功能有差异。
 1. 了解u-boot的功能。

## 3.3 中断、异常和系统调用比较
 1. 举例说明Linux中有哪些中断，哪些异常？

> 中断比如有系统需要响应外部设备，进行中断现在需要运行的程序，对于外部进行响应，此外还有进程调度等一系列的中断服务。
异常指的是应用程序出现了意想不到的行为，例如除零错误，数组访问越界等，通常我们在跑程序的时候出现的运行错误大多是这种异常引起的。

 1. Linux的系统调用有哪些？大致的功能分类有哪些？  (w2l1)
> linux的系统调用数目种类繁多，大致的分类如下：
进程管理：管理计算机运行的进程
文件系统管理：对于计算机的文件进行管理，可以进行文件查看复制等操作
内存管理：管理内存当中的东西，涉及页表等
socket控制：涉及多线程
用户管理：涉及多个用户切换，可以更好地保护用户隐私和安全

 
 1. 以ucore lab8的answer为例，uCore的系统调用有哪些？大致的功能分类有哪些？(w2l1)

> 我们打开lab8_answer里面的kern中的syscall系统调用文件里面的.c文件，里面的第161行到182行完整地记录了这个文件当中使用的系统调用，大约有22个，比如有SYS_exit，SYS_fork等，大致功能分类有如下几个方面：
文件操作：里面有pgdir，getdirentry等操作
进程调度：kill，wait，exec等操作
内存管理：read，write读写内存等操作
lab8中的完整版的系统调用如下：
[SYS_exit]              sys_exit,
    [SYS_fork]              sys_fork,
    [SYS_wait]              sys_wait,
    [SYS_exec]              sys_exec,
    [SYS_yield]             sys_yield,
    [SYS_kill]              sys_kill,
    [SYS_getpid]            sys_getpid,
    [SYS_putc]              sys_putc,
    [SYS_pgdir]             sys_pgdir,
    [SYS_gettime]           sys_gettime,
    [SYS_lab6_set_priority] sys_lab6_set_priority,
    [SYS_sleep]             sys_sleep,
    [SYS_open]              sys_open,
    [SYS_close]             sys_close,
    [SYS_read]              sys_read,
    [SYS_write]             sys_write,
    [SYS_seek]              sys_seek,
    [SYS_fstat]             sys_fstat,
    [SYS_fsync]             sys_fsync,
    [SYS_getcwd]            sys_getcwd,
    [SYS_getdirentry]       sys_getdirentry,
    [SYS_dup]               sys_dup,
 ```
  + 采分点：说明了ucore的大致数量（二十几个），说明了ucore系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.4 linux系统调用分析
 1. 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(w2l1)

> 本题必须要在linux的环境下面进行编写，windows会因为缺少必要的函数库而无法编译通过 

 ```
  + 采分点：说明了objdump，nm，file的大致用途，说明了系统调用的具体含义
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 
 ```
 
 1. 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(w2l1)
 
 ```
  + 采分点：说明了strace的大致用途，说明了系统调用的具体执行过程（包括应用，CPU硬件，操作系统的执行过程）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.5 ucore系统调用分析
 1. ucore的系统调用中参数传递代码分析。
 1. ucore的系统调用中返回结果的传递代码分析。
 1. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。

> 以下我们分析lab8的answer当中的kern文件当中的syscall文件中.c文件的几个系统调用的意义
sys_exit:引发一个系统退出的异常
sys_fork:为当前进程创造一个子进程
sys_wait：一个进程进行等待
sys_exec：这是可执行文件的映射的建立和读取
sys_yield：调用一个处理调用的内核函数
sys_kill：用于挂断一个进程
sys_getpid：用来返回当前进程的id
sys_putc：有关tcpip协议
sys_pgdir：显示页面的目录
sys_gettime：获得当前时间
sys_lab6_set_priority：调整lab6的优先级权限
sys_sleep：休眠状态
sys_open：打开文件
sys_close：关闭文件
sys_read：读取文件
sys_write：写文件
sys_seek：返回当前文件当下的读写位置
sys_fstat：和read，write类似
sys_fsync：将缓存区域的数据写入指定的设备
sys_getcwd：获取当前的工作目录
sys_getdirentry：获取文件对话框目录
sys_dup：用来“复制”一个打开的文件号，使两个文件号都指向同一个文件
 1. 以ucore lab8的answer为例，尝试修改并运行代码，分析ucore应用的系统调用执行过程。
 
## 3.6 请分析函数调用和系统调用的区别
 1. 请从代码编写和执行过程来说明。
   1. 说明`int`、`iret`、`call`和`ret`的指令准确功能
 

