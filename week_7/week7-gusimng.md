### 回收子进程：

内核并不是立即把终止的进程从系统中清除->终止但还未被回收的进程称为僵死进程（不运行，但仍消耗系统内存资源）(zombie)；init进程的PID为1，系统启动时由内核创建，它不会终止，是所有进程的祖先->内核会安排init进程去回收孤儿进程；waitpid、wait->P516;waitpid返回已终止子进程的PID时，这个进程已经被回收；

### 逐级缓存的思想。

（1）前有硬件上弥补CPU与内存间速度差距的SRAM高速缓存；

（2）现有软件上虚拟内存系统中页面的调度（缓存PTE的TLB、SRAM、DRAM、磁盘）（在硬件上缓存的不止指令和数据，还有虚拟寻址用的页表、页表条目、虚拟页）（页作为磁盘和较高层的主存之间的传输单位）（主存中的每个字节都有一个选自虚拟地址空间的虚拟地址和一个选自物理地址空间的物理地址）

（3）再细到对内存空闲块的管理，页是分类快速“逐级”索引的思想，划分大小类级别，维护一个空闲链表数组。