# lab2 report
 
###练习一：

我实现的first-fit算法主要还是基于所给的lab2程序，在这个程序当中我们需要修改的地方不太多，加起来大概也就是十几行的样子，我保留了default_init和init_memmap的代码，对于alloc和free进行了修改，我们设计的分配过程就是遍历链表寻找第一个可以装得下的空闲页随后进行相关处理，而如果是free要把页面进行插入，然后通过对于之前和它之后的页面进行处理以求得到相关的合并操作。

first-fit算法思路上大体是一致的，进一步的改进空间只能是在实现细节上面做文章，比如改进一些合并和释放的时候的更改属性的操作，另外可以由程序的局部性原理，使用下标结构数组代替链表进行实现（我在spoc的实现当中使用的是数组），节省时间提升访问效率。

###练习二：

实现思路：首先是要在一级页表也就是页表目录项当中查找需要的二级页表，如果二级页表不存在我们就尝试create这个项，如果create不成功就return null，如果可以创建就申请物理内存然后设置标志，按照注释提示对页表项进行清零，另外如果二级页表已经存在那么就计算地址即可。

PDX:二级页表在页目录项当中的index，对于ucore来讲可以链接到下一步的查找页表当中去，起到一个相互关联的作用。

PDE_ADDR：一个页表项在页目录表当中记录的位置

PTE_P：判定这个页表项是否存在，对于判定这个页表项有没有相应的合法映射有帮助

PTE_W：判定这个页表是否可以写

PTE_U：判定这个页表的使用的用户权限

PTE_PWT：判定这个页表在构建TLB的时候是不是支持拖后写

PTE_PCD：判定这个页表是不是不支持cache，，这一部分大多和cache的支持有关系

PTE_D：判定这个页是不是脏页，对于拖后写之后的页替换有帮助

PTE_A：判定这个也是不是可以被接受

PTE_PS：知道这个页的大小，了解size便于使用循环操作和进行地址的操作

PTE_MBZ:必须是0的bits有多少位

PTE_AVAIL：是否对于用户的应用程序可用，有些页表是存储了内核的内容，不能被用户态访问

出现了访问异常，硬件陷入了内核态，需要维护堆栈，记录程序现场，如果发现发生了异常嵌套，根据操作系统的种类不同，确定是系统崩溃以保护还是继续将异常优先级区分进行堆栈入栈出栈的操作，
硬件对于寄存器要注意处理，其他交给操作系统处理即可。

###练习三：

首先判断这个要移除的页是否存在，如果存在进行下一步的判断，找到这个页的映射项，降低这个页被引用的次数，减一即可，当这个时候，如果这个页的引用次数变成0的话我们就释放这个页，相应的我们要对于tlb进行修改。

这个page结构我认为是相当于一个内存存储的单位，这个page结构是和页表项，页目录项有着对应的关系，当查找到二级页表时需要分配一个页，我们就有使用alloc_page的函数对于页page进行分配，而在free的时候也有相应的操作，页目录的pgdir也是由page类型进行存储的，但是具体的数字对应关系我们并不能从代码当中获得。

我认为要让虚拟地址和物理地址相等的话大体的思路有两种：第一种是更改entry文件和映射机制，通过一些加减操作让当所有映射都被更新之后保证最终的va和pa相等，第二种进入内核态（防止有的页在用户态下不能访问）是遍历所有的页，将对应的物理地址进行修改，这种方法耗时太多……不过不太理解为什么要让逻辑地址和物理地址相等？有页表就有它存在的合理性，如果使用一些方法让逻辑地址和物理地址相等，那么付出的代价我认为是高于维护查询页表的。
