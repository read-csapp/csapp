
- C源码 通过*编译器*变为  汇编代码   通过*汇编器*变为  目标代码
- （目标代码 + 静态连接库）通过*链接器* 变为可执行代码
- 可执行代码 通过*加载器* 加载到内存中，CPU去读取执行

在Linux平台下可执行文件和目标文件使用的是叫ELF格式的文件，这里面主要包含：
- .text: 代码段和指令段
- .data: 输出化好的数据
- .rel.text: 重定位表，即不知道该跳转到哪里，就放在这里面
- .symtab: 符号表，函数名和地址的映射

1. *链接器*根据输入的所有文件，把所有符号表里面的信息收集起来构成一个全局符号表。
2. *链接器*再根据重定位表，把所有不确定要跳转的地址的代码，根据符号表里面的地址进行修正
3. *链接器*最后把所有的目标文件的对应段进行一次合并，变成最终的可执行代码。


windows下面可执行文件的格式是PE（Portable Executable Format），因为可执行文件的格式和linux的ELF的格式都不一样，所以不能相互解析
