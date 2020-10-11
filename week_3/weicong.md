第三章总结
1. 程序编码过程
    1. 程序编码过程 `linux> gcc -Og -o p p1,c p2,c` <br> 
    这个过程中 -Og告诉编译器使用会生成符合原始C代码整体结构的机器码的优化等级<br>
    首先c预处理器扩展源代码：插入所有`#include`指令指定的文件，并且扩展所有用`#define`声明指定的宏<br>
    第二步，编译器产生两个源文件的汇编代码 `p1.s,p2.s`
    第三部汇编起将汇编码转换成二进制目标代码文件 `p1.o,p2.o`(目标代码是机器码的一种形式)
    最后一步，连接器将两个目标代码与实现库函数的代码结合，产生最终可执行文件(可以用`-o`指定输出文件的位置与名字)
    2. 汇编中提供，程序计数器，整数寄存器文件，条件码寄存器，向量寄存器
    3. 在我的os系统上，编译mstore.c文件后出现许多与教科书上不符合的汇编指令。后来了解到是`指导编译器与连接器工作的伪指令` 
    ```
   	.section	__TEXT,__text,regular,pure_instructions
   	.build_version macos, 10, 14
   	.globl	_multstore              ## -- Begin function multstore
   	.p2align	4, 0x90
   _multstore:                             ## @multstore
   	.cfi_startproc
   ## %bb.0:
   	pushq	%rbp
   	.cfi_def_cfa_offset 16
   	.cfi_offset %rbp, -16
   	movq	%rsp, %rbp
   	.cfi_def_cfa_register %rbp
   	pushq	%rbx
   	pushq	%rax
   	.cfi_offset %rbx, -24
   	movq	%rdx, %rbx
   	callq	_mult2
   	movq	%rax, (%rbx)
   	addq	$8, %rsp
   	popq	%rbx
   	popq	%rbp
   	retq
   	.cfi_endproc
                                           ## -- End function
   
   .subsections_via_symbols

   ```
   存在许多`.`开头的指令。
2. 默认格式为`ATT` 是gcc与objdump的默认格式，intel有独特的格式，有忽略了指令大小的后缀等
3. 数据格式，intel称为，16位为"字"，32位为"双字"，64位为"四字" 记录一下c语言基本数据类型对应的x86-64表示

  |c声明|Intel数据类型|汇编代码后缀|大小（字节）|
  |---|---|---|---|
  |char|字节|b|1|
  |short|字|w|2|
  |int|双字|l|4|
  |long|四字|q|8|
  |char*|四字|q|8|
  |float|单精度|s|4|
  |double|双精度|l|8|
  
   `x86家族的微处理器历史上实现过一种特殊的80位（10字节）浮点格式运算，在c中用long double来指定格式`
   
  大多数gcc的汇编带阿妈指令都有一个字符的后缀表示操作数的大小
4. 访问信息
   1. 一个x86-64的中央处理单元包含一组16个存储64位值的`通用目的寄存器` ,他们的名字一%r开头<br>特别的计数，%rsp 栈指针
   2. 操作数指令，分三种。
        1. 立即数：用来表示常数
        2. 寄存器：表示某个寄存器的内容
        3. 内存引用：它会根据计算出来的地址，访问某个内存位置
   3. 数据的传送。`mov`指令，分别有`movb,movw,movl,movq,movabsq` 分别表示1，2，4，8字节的move指令与传送绝对的四字。
   4. 传送示例：
   
c语言
  ```
    long exchange(long *xp,long y)
{
    long x = *xp;
    *xp = y;
    return x;
}
```
汇编
```
_exchange:                              ## @exchange
	.cfi_startproc
## %bb.0:
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register %rbp
	movq	(%rdi), %rax
	movq	%rsi, (%rdi)
	popq	%rbp
	retq
```

可以明确的看到采用了movq来进行移动内存值与立即数

* 压栈与弹出栈数据
    * 操作指令 `push pop`
    * `pushq %rbp` 等价于 `subq %8,%rsp movq %rbp,(%rsp)`
* 算数和逻辑操作 
    * `lea inc dec neg not add sub imul xor or and sal shl saar shr`分别为`加载有效地址，加1，减1，取负，取补，加，减，乘，除异或，或，与，左移，左移（等同于SAL），算数右移，逻辑右移`
    * 加载有效地址 `leaq` 是一个movq的变形。
    * 一元与二元，一元：只有一个操作数，即是源又是目的。`inc dec neg not` 都为一元操作，`and sub imul xor or and`都为二元操作
* 控制 ---- 条件语句，循环语句，分支语句等
    * 条件吗
    
| | | |
|---|---|---|
| CF|进位标示 |最近的操作使最高位产生了进位，可用来检查无符号操作的溢出|
| ZF|零标示 |最近操作得出的结果位0|
| SF|符号标示 |最近的操作得出的结果为负数|
| OF|溢出标示 |最近的操作导致一个补码溢出----正溢出或者负溢出|
控制条件的指令
* `cmpb cmpw cmpl cmpq` 为比较
* `sete setne ` 相等/零   ， 不等/非零
* `sets setns`  负数，非负数
* `setg setge`  大于，大于等于（有符号）
* `setl setle`  小于，小于等于（有符号）
* `seta setae`  超过，超过或相等（无符号）
* `setb setbe`  低于，低于或等于（无符号）
  
jump指令
* 跳转指令，通常伴随上一条指令的判断。
* whele 与if else 与 switch等语句都是通过jump来实现
* jump与goto模式相同
