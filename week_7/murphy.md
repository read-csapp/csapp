# Virtual Memory

## Concepts

虚拟化的概念在计算机科学中是非常重要的，当你准备虚拟化一个资源的时候，你就要向请求该资源的用户展现这个资源的不同类型的视图，这种视图通常是资源的某种抽象。

我们可以通过干预和介入对资源的访问过程来完成虚拟化的过程。

磁盘是一种经典的例子。尽管磁盘的结构复杂，但是磁盘控制器给上层应用的视图是一系列的逻辑块。磁盘控制器就是拦截了内核发送的逻辑块转换为了实际的物理地址。

对于访问内存的指令的拦截，是由一个叫MMU的内存管理单元所完成的。

当CPU执行一个指令的时候，其访问的虚拟地址，这个虚拟地址会被MMU转换为实际的物理地址，然后对应我们想要的数据的物理地址。

使用MMU管理内存的优点？

- 更为有效的使用主存（你可以将虚拟内存视为存储在磁盘上的字节序列，而主存DRAM就像是虚拟内存的缓存)
- 更为简单的管理内存
- 地址空间独立，防止其他进程访问，保护内存。

虚拟地址空间通常比物理地址空间大的多，且对于所有进程中，其看到自己的虚拟空间是相同的。这种简单易操作的模型正是MMU所想提供的。

虚拟内存中将数据分成页。每一个页都将分配一个数字，这些页面的一部分被存储在了物理的DRAM中，而有一些则没有。系统中存在map帮我们缓存虚拟内存到DRAM的映射。

DRAM在未命中的情况下就得去磁盘里面去取数据，这会是巨大的消耗。这时候，块的大小变成了一个关键的议题，块太小会导致经常性的未命中，而块太大则会导致内存不够用，也会导致未命中。

虚拟内存系统很少直接写入磁盘，而是会尽可能的延缓这个过程而增加运行速度。

负责映射的map称之为page table（PTE），存储在**内存**中，由内核维护，每个进程都有，key是虚拟页的编号，而value是其存储的物理地址。

如果未命中，就会导致缺页异常，然后从磁盘里把数据读到内存中，之后重新执行。如果内存满了，就要将一个内存页删掉再读。

虚拟内存找到对应物理地址的方法和高速缓存相近，是由虚拟页的号码和在此页中的offset所组成的，虚拟块中的偏移量和物理块中的offset是相等的。除此以外还有个valid记录此条信息是否合法。

因为page table是存储在内存中的，内存页毕竟还是不够快，所以我们很明显可以用SRAM缓存来加速这个过程，而这块SRAM就是TLB。

尽管本质上目前计算机的最高寻址位是2的48次方，但是这个数量依旧是过于庞大，对于一页2的12次方大小的页，需要2的36次方条记录才可以完成完备的映射。

所以在现代计算机中使用了多页表的方式，不存储大多数不用的地址空间，从而达到减少内存使用的目的。

