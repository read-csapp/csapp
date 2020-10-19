### 存储器结构

#### SRAM和DRAM
SRAM需要的晶体管比较多，一个bit需要6～8个晶体管。相同的物理空间下，存储的数据有限，但是因为SRAM的电路简单所以访问速度快


DRAM存储一个bit只需要一个晶体管和一个电容即可，所以存储密度比较大。因为数据是存储在电容里面的，电容会不断漏电，所以需要定时刷新，这么来的动态。


DRAM的数据访问电路和刷新电路都比SRAM长，所以访问延时也比较长。


SRAM一般是CPU处理器使用，DRAM一般是内存使用。
#### L缓存
L1缓存的是每个CPU独占，分为指令缓存和数据缓存

L2缓存也是每个CPU独占，但是不在CPU的内部会满一些

L3是多核CPU共享的缓存

#### 举例：Intle i5-8265U

1. 每个核心有32K的cache，一共4 * 32 = 128K
2. 指令和数据cache采用8路组相连的放置策略
3. 每个核有256K的L2 cache，L2 cache采用4 路组相连的放置策略。
4. 一块多个核心共用的 12MB 的 L3 Cache，采用的是 12 路组相连的放置策略。

https://colin-scott.github.io/personal_website/research/interactive_latency.html
