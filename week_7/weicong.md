9.虚拟存储器
为了更加有效地管理存储器且少出错，现代系统提供了对主存的抽象概念，叫做虚拟存储器(VM）。
* 		虚拟存储器是硬件异常，硬件地址翻译，主存，磁盘文件和内核软件的完美交互。
* 		为每个进程提供一个大的，一致的和 私有的地址空间。
* 		提供了3个重要能力。
    * 		将主存看成磁盘地址空间的高速缓存。 
        * 		只保留了活动区域，并根据需要在磁盘和主存间来回传送数据，高效使用主存。
    * 		为每个进程提供一致的地址空间
        * 		简化存储器管理
    * 		保护了每个进程的地址空间不被其他进程破坏。
* 		程序员为什么要理解它？
    * 		虚拟存储器是中心的。 
        * 		遍布在计算机系统所有层次，硬件异常，汇编器，连接器，加载器，共享对象，文件和进程中扮演重要角色。
    * 		虚拟存储器是强大的。
        * 		可以创建和销毁存储器片(chunk)
        * 		将存储器片映射到磁盘文件的某个部分。
        * 		其他进程共享存储器。
        * 		例子 
            * 		能读写存储器位置来修改磁盘文件内容。
            * 		加载文件到存储器不需要显式的拷贝。
    * 		虚拟存储器是危险的
        * 		引用变量，间接引用指正，调用malloc动态分配程序，就会和虚拟存储器交互。
        * 		如果使用不当，将遇到复杂危险的与存储器有关的错误。
        * 		例子 
            * 		一个带有错误指针的程序可以立即崩溃于段错误或者保护错误。
            * 		运行完成，却不产生正确结果。
* 		本章从两个角度分析。
    * 		虚拟存储器如何工作。
    * 		应用程序如何使用和管理虚拟存储器。
虚拟内存作为缓存工具
在讲述这一小章之前，必须交代一下我对虚拟存储器概念的存疑。 
原本我以为虚拟存储器=虚拟内存。 
以下是虚拟内存的定义
虚拟内存是计算机系统内存管理的一种技术。它使得应用程序认为它拥有连续的可用的内存（一个连续完整的地址空间），而实际上，它通常是被分隔成多个物理内存碎片，还有部分暂时存储在外部磁盘存储器上，在需要时进行数据交换
而在下面的定义我们可以看到CSAPP中认为虚拟存储器是存放在磁盘上的。 
在此，我们姑且当做两者是不同的东西，以后有更深刻的理解，再思考。
缺页
在虚拟存储器的习惯说法中，DRAM缓存不命中称为缺页。
处理过程如下：
* 		读取虚拟地址所指向的PT。
* 		读取PTE有效位，发现未被缓存，触发缺页异常。
* 		调用缺页异常处理程序
    * 		选择牺牲页。
    * 		如果牺牲页发生了改变，将其拷贝回磁盘(因为是写回)
    * 		需要读取的页代替了牺牲页的位置。
    * 		结果：牺牲也不被缓存，需要读取的页被缓存。
* 		中断结束，重新执行最开始的指令。
* 		在DRAM中读取成功。
虚拟存储器是20世纪60年代发明的，因此即使与SRAM缓存使用了不同的术语。
* 		块被称为页。
* 		磁盘和DRAM之间传送页的活动叫做交换(swapping)或者页面调度(paging)。
* 		有不命中发生时，才换入页面，这种策略叫做按需页面调度(demand paging)。 
    * 		现代系统基本都是用这个。
