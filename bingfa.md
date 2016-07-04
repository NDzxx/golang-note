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
这里我们需要做同步, 所谓同步, 就是主线程去等待子线程完成(go里的线程其实是协程, 协程你可以认为是轻量级的线程).

线程间通信模型基本上常用的也就是两种,共享内存和消息队列.  
后者目前看来比较流行, 比如akka(actor模型), zmq(消息队列模型)等 都是基于消息队列的并发模型.
#channel
go也是采用了消息队列的方式, 这里就是channel.

channel的申明形式:

var chanName chan ElementType

ElementType是该chan能够支持的数据类型.

chan的初始化有两种:

ch := make(chan int)

和

ch := make(chan int,capacity)  
前者写入和读取是阻塞的, 后者自带了一个buffer,
如果没有达到capacity, 是非阻塞的, 达到capacity才会阻塞. 读取的话, 如果为buffer空,
会阻塞.

chan写入数据

ch <- value

chan读取数据

value : = <-ch  
对于带buffer的chan也可以用for和range读取:  
```
for i := range ch {
 fmt.Println("Received:", i)
}
```
##单向channel

