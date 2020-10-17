- 立即数（immediate），用来表示一个具体数 例如 $5
- 寄存器（ra），表示某个寄存器的内容
- 绝对寻址， 直接写某个地址
---

- C语言的指针其实就是地址，引用指针，操作就是把指针地址放在寄存器中，然后再内存中引用该地址。
- 局部变量通常放在寄存器中、而非内存，因为读寄存器会比读内存快很多，

---

压入/弹出栈操作
- push 
将栈指针减去相应的字节数（例如pushq 即减8)，然后将值放入栈顶
- pop
读值，将栈指针减去相应的字节数

--- 

switch / if

对比汇编代码 知道if需要一一比较、所以在平常设计中需要将最常出现的列在第一位、尽早结束if
switch语句会使用跳转表、所以会直接跳转到相应的case，故而速度要快很多，