# 工作中遇到的浮点数问题


### 前端浮点数相加导致显示问题

![](./fe-float.jpeg)

这个是前端浮点数相加常会遇到的问题：
```console
0.3 + 0.6
# 0.8999999999999999

0.1 + 0.2 
# 0.30000000000000004


```

### java float的使用

```java
public class FloatPrecision {
  public static void main(String[] args) {
    float a = 20000000.0f;
    float b = 1.0f;
    float c = a + b;
    System.out.println("c is " + c);
    float d = c - a;
    System.out.println("d is " + d);
  }
}
```

outut:
```shell
c is 2.0E7
d is 0.0
```

循环加2000w次
```java
public class FloatPrecision {
  public static void main(String[] args) {
    float sum = 0.0f;
    for (int i = 0; i < 20000000; i++) {
      float x = 1.0f;
      sum += x;      
    }
    System.out.println("sum is " + sum);   
  }  
}
```
output:
```shell
sum is 1.6777216E7
```

解决办法：
```shell
public class KahanSummation {
  public static void main(String[] args) {
    float sum = 0.0f;
    float c = 0.0f;
    for (int i = 0; i < 20000000; i++) {
      float x = 1.0f;
      float y = x - c;
      float t = sum + y;
      c = (t-sum)-y;
      sum = t;      
    }
    System.out.println("sum is " + sum);   
  }  
}
```



