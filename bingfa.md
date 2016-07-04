# go并发编程
并发不是并行. 并发比并行更加优秀.   
并发是时间片轮换, 并行是多核计算. 事实上, 并行可以由并发指定到多个cpu执行.  

```
package main

import (
	"fmt"
)

func Add(x, y int) {
	z := x + y
	fmt.Println(z)
}

func main() {
	for i := 0; i < 10; i++ {
		go Add(i, i)
	}
}
```
我们发现什么都没有输出, 这是因为go是异步执行的, main不会等Add完成, 它会继续执行下去, 于是发生了main
函数先结束, 于是进程就结束了.