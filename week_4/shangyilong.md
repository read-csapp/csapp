### 数组和指针的关系
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

