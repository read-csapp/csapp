# ***\*第一章\****

## ***\*程序编译过程\****

预处理器：读取头文件，插入到程序文本中

编译器：翻译成汇编语言

汇编器：翻译成机器语言指令，打包成可重定位目标程序

链接器：合并调用的库.o文件

 

![img](file:///C:\Users\red\AppData\Local\Temp\ksohtml7888\wps1.jpg) 

 

文件：对 I/O 设备的抽象

虚拟内存：对程序存储器的抽象

进程：对一个正在运行程序的抽象

虚拟机：对整个操作系统的抽象

 

## ***\*进程和线程\****

进程是程序在操作系统中的一次执行过程，系统进行资源分配和调度的一个独立单位。

线程是进程的一个执行实体,是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位。

一个进程可以创建和撤销多个线程;同一个进程中的多个线程之间可以并发执行。

## ***\*并发和并行\****

并发：同一时间段内执行多个任务。（吃饭和喝汤）

并行：同一时刻执行多个任务。（吃饭和看电视）

 

# ***\*第二章\**** 

## ***\*大端法和小端法\****

一般内存表示为小端法，即低位内存地址在前。

大端法：高位字节在前，低位字节在后，这是人类读写数值的方法。

小端法：低位字节在前，高位字节在后，即以0x2211形式储存。

 

## ***\*优先级\****

位移运算优先级低于加减运算

优先级

乘除>加减>位移运算>位运算>逻辑运算

 

## ***\*位移运算\****

逻辑右移：右移后高位补零

算数右移：右移后高位补最高有效位的值