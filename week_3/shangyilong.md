## 

这一章需要记的东西比较多，


现在用的汇编代码格式都是ATT格式非Intel格式的代码，Intel和ATT代码格式有下面的不同：
1. Intel代码省略指令大小的后缀，push而不是pushq，mov而不是movq
2. Intel省略寄存器名字前面的%，比如使用rbx，不是用%rbx
3. Intel代码‘QWORD PTR [rbx]’而不是使用(rbx)表示地址
4. 多个操作数的情况下，列出的操作数和顺序相反


### 指令类别

#### 算术类指令

一元操作
- INC D：加一
- DEC D：减一
- NEG D：取负
- NOT D：取反

两元操作（D = D op S）
- ADD S, D
- SUB S, D
- IMUL S, D
- XOR S, D
- OR S, D
- AND S, D


#### 逻辑类指令

移位操作
- SAL k, D: 左移 
- SHL k, D: 左移
- SAR k, D: 算数右移
- SHR k, D: 逻辑右移


#### 数据传输类指令
加载有效地址：
- leaq S,D

移动：
- MOVZ把目的中剩余的字节填充为0
- MOVS按照最高位进行复制

出栈和入栈(栈顶指针是rsp)
- pushq S
- popq D




#### 条件分支类指令

条件码
- CF：进位标志
- ZF：零标志
- SF：符号标志，操作结果是否为负数
- OF：溢出标志，正溢出/负溢出


比较(只设置条件码，不改变寄存器)：
- cmp S1, S2
- test S1, S2

设置（通常和比较/条件联合使用）
比如：
```shell
# %rsi -> a
# %rdi -> b
comp:
  cmpq: %rsi, %rdi
  setl %al
  ret
```
表示的是如果a < b那么设置%al为0/1
