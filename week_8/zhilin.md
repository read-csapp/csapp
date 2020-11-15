## 虚拟内存
虚拟内存不只是“用磁盘空间来扩展物理内存”的意思，把内存扩展到磁盘只是使用虚拟内存技术的一个结果，
它的作用也可以通过覆盖或者把处于不活动状态的程序以及它们的数据全部交换到磁盘上等方式来实现。
对虚拟内存的定义是基于对地址空间的重定义的，即把地址空间定义为“连续的虚拟内存地址”，
以借此“欺骗”程序，使它们以为自己正在使用一大块的“连续”地址。
是现代系统提供的一种对主存的抽象概念，
它将主存看成是一个存储在磁盘上的地址空间的高速缓存，在主存中只保存活动区域按需在磁盘和主存之间来回传送数据；为每个进程提供了一致的地址空间，简化了内存管理；并且保护每个进程的地址空间不被其他进程破坏。

### 内存映射(memory mapping)
Linux通过将一个虚拟内存区域与一个磁盘上的对象(object)关联起来，已初始化这个虚拟内存区域的内容，这个过程称为内存映射。虚拟内存区域可以映射到两种 类型的对象中的一种:

Linux 文件系统中的普通文件: 一个区域可以映射到一个普通磁盘文件的连续部分，例如一个可执行目标文件。

文件区(section)被分成页大小的片，每一片包含一个虚拟页面的初始内容。如果区域比文件区要大，那么就用零来填充这个区域的余下部分。

匿名文件：一个区域也可以映射到一个匿名文件，匿名文件是由内核创建的，包含的全是二进制零。

在任何时刻，交换空间都限制着当前运行着的进程能够分配的虚拟页面的总数

### 写时复制
只要没有进程试图写自己的私有区域，它们就可以在继续共享物理内存中所存储的共享对象的一个单独副本。然而，只要有一个进程试图写私有区域内的某个页面，那么这个写操作就会触发一个保护故障(Protection failure). 当故障处理程序注意到保护异常是由于进程试图写私有的写时复制区域中的一个页面而引起的，它就会在物理内存中创建这个页面的一个新副本，更新页表条目指向这个新的副本，然后恢复这个页面的可写权限。当故障处理程序返回时，CPU重新执行这个写操作，现在在新创建的页面上这个写操作就可以正确执行了。

### 关于内存的常见的错误示例
间接引用坏指针,读取未初始化的内存，允许栈缓冲区溢出，假设指针和它们指向的对象大小相同，引用指针而不是它所指向的对象，误解指针运算，引用不存在的变量，以及引起内存泄漏

指针运算
指针的算术操作是以它们指向的对象的大小为单位来进行计算的。这个单位的大小并不一定是字节。例如，下面函数的目的是扫描一个int的数组，并返回一个指针。指向val首次出现的位置。

#### 动态内存分配

动态内存分配器，将堆作为大小不同的块集合来维护。每个块要么是已分配的，要么是空闲的。

分配器核心目标：1.最大化吞吐率 2.最大化内存利用率

#### 垃圾收集

Mark&Sweep收集器：由标记和清楚阶段组成

