第二章总结
1.c和C++都支持有符号数和无符号数，而Java只支持有符号数,但是java也可以自己做无符号数~但是必须浪费一位。C语言标准并没有要求有符号数采用补码的形式存储，C库中的<limits.h>头文件定义了一组常量，来限定编译器运行的这台机器的不同整型的取值范围。
2.多字节对象都被存储为连续的字节序列，对象的地址为所使用字节中的最小地址
3.有符号与无符号之间转换，正数与负数转换规则不同，因为首位的正负权重不同，导致需要分段处理。
4.在标识int32最小值时，必须携程-2147483647-1，因为编译器处理一个-x的表达式，首先读取x再取反。但是21474836478已经大于31位。
5.乘以2的幂:乘法除法可以优化成为移位
6.浮点数，0.01与0.010都表示4分之1，但是占位不同。二进制中无法准确标识无限循环小数
6，向上，下，偶，零取舍的概念
7.编译器优化加法会试图减少一次运算 
8.c++中浮点数 与int double转换，会有可能溢出与舍入 