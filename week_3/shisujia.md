* 第三章主要是将C程序翻译成汇编语言，然后讲解的程序是如何运行的，也介绍了数据结构在内存中的存储和表现方式，就更加偏向于语言的源码我感觉，大多数我是没太理解的

* 自己接触比较多的就是for循环和switch语句，在日常写代码过程中遇到的一些问题也可以和大家分享一下

* 例如这段代码

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func main() {
  	cnt := 3
  	for cnt >= 0 {
  		num := 1
  		switch num {
  		case 1:
  			fmt.Println(1)
  			break
  		default:
  			fmt.Println(2)
  		}
  		cnt--
  	}
  }
  ```

* 现象：期望输出一个 1，但实际上会输出的是 4 个 1
* 分析：在 Golang 语言中需要特别注意：for 与 switch 结合使用的陷阱，break 语句只会跳出 switch 代码块
