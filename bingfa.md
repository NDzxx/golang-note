# go并发编程
并发不是并行. 并发比并行更加优秀.   
并发是时间片轮换, 并行是多核计算. 事实上, 并行可以由并发指定到多个cpu执行.  
numcpu := runtime.NumCPU()
runtime.GOMAXPROCS(numcpu) //设置使用多少个cpu核心


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
顾名思义，单向channel只能用于发送或者接收数据。channel本身必然是同时支持读写的，
否则根本没法用。假如一个channel真的只能读，那么肯定只会是空的，因为你没机会往里面写数
据。同理，如果一个channel只允许写，即使写进去了，也没有意义，因为没有机会读取里面
的数据。所以的单向channel概念，其实只是对channel的一种使用限制。
```
var ch1 chan int // 双向
var ch2 chan<- float64// 只写
var ch3 <-chan int // 只读
```
关闭channel  
close(ch)  
如何判断一个channel是否已经被关闭？我们可以在读取的时候使用多重返回值的方式：

x, ok := <-ch
例子：
```
package main

import (
  "fmt"
)

func RandomSignal(ch chan int)  {
  for i:= 0; i <= 9; i++{
    select{
      //写入1事件
      case ch <- 1:
      //写入0事件
      case ch <- 0:
    }
  }
  close(ch)
}

func main() {
  //var ch chan int = make(chan int)效果一样
  //但是有些微妙的不同
  var ch chan int = make(chan int,1)
  go RandomSignal(ch)
  for value := range ch{
    fmt.Println(value)
  }
}
```
##超时处理机制
例子：  
```
 timeout:= make(chan bool,1)
 go func() {
	time.Sleep(1e9)//等待1秒钟
	timeout <- true
}()

select {
	case <-ch:
	//从ch中读取到数据
	case <-timeout:
	//一直没有从ch中读取到数据，但是从timeout中读取到了
	
}
```
##全局唯一性操作
```
var Once synv.Once
func DoPrint() {
  once.Do(setup)//保证全局范围内只调用setup一次
  print(a)
}
```
