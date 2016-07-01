# 错误和异常
#①error接口
Go语言中的error类型实际上是抽象了Error()方法的error接口
```
type error interface {
    Error() string
}
```
Go语言使用该接口进行标准的错误处理。

对于大多数函数，如果要返回错误，大致上都可以定义为如下模式，将error作为多种返回
值中的最后一个，但这并非是强制要求：
```
func Foo(param int)(n int, err error) {
    // ...
}
调用时的代码建议按如下方式处理错误情况：
n, err := Foo(0)
if err != nil {
    // 错误处理
} else {
    // 使用返回值n
}
```
完整例子
```
package main

import (
    "fmt"
)

//自定义错误类型
type ArithmeticError struct {
    error   //实现error接口
}

//重写Error()方法
func (this *ArithmeticError) Error() string {
    return "自定义的error,error名称为算数不合法"
}

//定义除法运算函数
func Devide(num1, num2 int) (rs int, err error) {
    if num2 == 0 {
        return 0, &ArithmeticError{}
    } else {
        return num1 / num2, nil
    }
}
func main() {
    var a, b int
    fmt.Scanf("%d %d", &a, &b)

    rs, err := Devide(a, b)
    if err != nil {
        fmt.Println(err)
    } else {
        fmt.Println("结果是：", rs)
    }
}
```
运行，输入参数6 3(正确的情况)：
```
6 3
结果是： 2
```
输入6 0
```
6 0
自定义的error,error名称为算数不合法
```
##②defer--延迟语句
在Go语言中，可以使用关键字defer向函数注册**退出调用**，即主调函数退出时，defer后的函数才会被调用。
defer语句的作用是不管程序是否出现异常，均在函数退出时自动执行相关代码。

当函数执行到最后时，这些defer语句会按照逆序执行，最后该函数返回。
```
package main

import (
    "fmt"
)

func main() {
    for i := 0; i < 5; i++ {
        defer fmt.Println(i)
    }
}
```
结果：
```
4
3
2
1
0
```

defer语句在声明时被加载到内存(多个defer语句按照FIFO原则) ，加载时记录变量的值，而在函数返回之后执行。看下面的例子：  
例子1：defer语句加载时记录值
```
package main

import (
	"fmt"
)
func f1() {
    i := 0
    defer fmt.Println(i) //实际上是将fmt.Println(0)加载到内存
    i++
    return
}
func main() {
    f1()//结果 0
}
```
例子2：在函数返回后执行
```
package main

import (
	"fmt"
)
func f2() (i int) {
    var a int = 1
    defer func() {
        a++
        fmt.Println("defer内部", a)
    }()
    return a
}
func main() {
    fmt.Println("main中", f2())
}
```
