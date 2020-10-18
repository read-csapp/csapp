jump
jump是跳转指令
大家都知道计算机执行程序是从上到下逐行逐句的执行的，跳转指令可以让程序跳段执行。
那么jump是随意跳的吗？肯定不是，如果是的话那我们直接定义一个指令直接操作IP寄存器不就得了，jump指令是根据运算结果来进行跳转的，例如c预言的判断语句就会被翻译成jump语句。
大体步骤如下：CPU计算，根据计算结果改变标识位，根据标识位进行跳转。
四个标识位：CF，ZF，SF，OF(这些标识位是存在于EFLAGS寄存器中的，该寄存器还有其他标识位，我们只需要了解这四个即可)
jump指令用法：
直接跳转：
jmp <label>
Example
jmp mylabel  ---- 跳转到mylabel定一段指定代码

刚刚不是才说jump指令是根据计算机计算结果，然后自动改变标识位，在根据标识位进行跳转的吗？
别急，这是直接跳转命令，接下来为你介绍条件跳转命令。
je <label> (jump when equal)
jne <label> (jump when not equal)
jz <label> (jump when last result was zero)
jg <label> (jump when greater than)
jge <label> (jump when greater than or equal to)
jl <label> (jump when less than)
jle <label>(jump when less than or equal to)

Example
cmp eax, ebx                               ;这个操作(计算)改变了标识位
jle done  ,                                     ;如果eax中的值小于ebx中的值，跳转到done指示的区域执行，否则，执行下一条指令。

call，ret
call 和 ret 指令分别实现子程序的调用和返回。
用call指令直接跳转到子程序开始的地方，这时候就有同学问了，这不跟jump指令一样吗？
是的，一样的，只是call指令还多做了现场保护的工作，而ret指令就是恢复被保护的现场。
至于什么是现场保护和现场恢复先不用关心，文章下面讲解，现在只需要知道call和ret是做这个的，先对他们有个概念。

call <label>
ret



AT&T常用汇编指令
数据传送指令
指令	效果	描述
movl S,D	D <-- S	传双字
movw S,D	D <-- S	传字
movb S,D	D <-- S	传字节
movsbl S,D	D <-- 符号扩展S	符号位填充(字节->双字)
movzbl S,D	D <-- 零扩展S	零填充(字节->双字)
pushl S	R[%esp] <-- R[%esp] – 4;M[R[%esp]] <-- S	压栈
popl D	D <-- M[R[%esp]]；R[%esp] <-- R[%esp] + 4;	出栈
