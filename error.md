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
##②defer--延迟语句(先进后出FILO)
在Go语言中，可以使用关键字defer向函数注册**退出调用**，即主调函数退出时，defer后的函数才会被调用。
简单的说：defer是在return之前执行的。这个在 官方文档中明确说明了的
defer语句的作用是不管程序是否出现异常，均在函数退出时自动执行相关代码。通常用于释放资源或者错误处理。

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
例子3：defer语句会读取主调函数的返回值，并对返回值赋值.(注意和例子2的区别)
```
func f3() (i int) {
    defer func() {
        i++
    }()
    return 1
}
func main() {
    fmt.Println(f3())//其结果竟然是2. 读取1再++
}
```
实际使用例子  
例如关闭文件句柄：
```
srcFile,err := os.Open("myFile")
defer srcFile.Close()
```
关闭互斥锁：
```
mutex.Lock()
defer mutex.Unlock()
```
上面例子中defer语句的用法有两个优点：

1.让设计者永远也不会忘记关闭文件，有时当函数返回时常常忘记释放打开的资源变量。

2.将关闭和打开靠在一起，程序的意图变得清晰很多。
下面看一个文件复制的例子：
```
package main

import (
    "fmt"
    "io"
    "os"
)

func main() {
    copylen, err := copyFile("dst.txt", "src.txt")
    if err != nil {
        return
    } else {
        fmt.Println(copylen)
    }

}

//函数copyFile的功能是将源文件sec的数据复制给dst
func copyFile(dstName, srcName string) (copylen int64, err error) {
    src, err := os.Open(srcName)
    if err != nil {
        return
    }
    //当return时就会调用src.Close()把源文件关闭
    defer src.Close()
    dst, err := os.Create(dstName)
    if err != nil {
        return
    }
    //当return是就会调用src.Close()把目标文件关闭
    defer dst.Close()
    return io.Copy(dst, src)
}
```
**滥用defer可能导致性能问题：**

##defer-panic-recover
当在一个函数执行过程中调用panic()函数时，正常的函数执行流程将立即终止，但函数中
之前使用defer 关键字延迟执行的语句将正常展开执行，之后该函数将返回到调用函数，并导致
逐层向上执行panic流程，直至所属的goroutine 中所有正在执行的函数被终止。错误信息将被报
告，包括在调用panic()函数时传入的参数，这个过程称为错误处理流程

```
package main


import "fmt"
func main() {
	defer func() {     //必须要先声明defer，否则不能捕获到panic异常
		fmt.Println("c")
        //调用recover函 数将会捕获到当前的panic
		if err := recover(); err != nil {
			fmt.Println(err)    //这里的err其实就是panic传入的内容，55
		}

		fmt.Println("d")
	}()
	f()
}


func f() {
	fmt.Println("a")
	panic(55)
	fmt.Println("b")

	fmt.Println("f")
}

```
