### 数组和指针的关系
数组A[i]和*(A + i)是一样的，当时有困惑，为什么会是一样的呢为什么没有乘类型的size呢，就写了下面的程序，当然也是习题的程序。
```c
#include <stdio.h>

int main()
{
  char a = '0';
  char *aa = &a;
  int *b = (int*)aa + 7;
  int *c = (int*)(aa + 7);
  printf("%d, %d, %d\n", c, b, aa);
  return 0;
}
```
上面代码输出：
```
-2007374258, -2007374237, -2007374265
```

