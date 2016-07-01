# 程序控制
- switch语句  
switch语句不需要在每个case地下写break,默认就是执行break.  
如果要执行多个case, 在case最后加入fallthrough.  
条件表达式不限制为常量或者整数.单个case自然可以有多个结果可以选.  
  ```  
  package main

  import "fmt" //引入依赖包

  func test(a int) {
      switch {
      case a < 0:
          fmt.Println("hello")
      case a == 10:
          fmt.Println("a==10")
          fallthrough
      case a > 10 && a < 100:
          fmt.Println("world")
          break
      default:
          fmt.Println("nima")
      }
  }

  func main() {
      test(-1)
      test(10)
      test(15)
      test(100)
  }
  ```
结果
```
hello
a==10
world
world
nima
```
- 循环 (go语言只有for循环)
  ```
  package main

  import "fmt" //引入依赖包

  func main() {
      sum := 0
      for i := 0; i <= 100; i++ {
          sum += i
      }
      fmt.Println(sum)

      sum = 0
      i := 0
      for(i <= 100){
        sum += i
        i++
      }
      fmt.Println(sum)
  }
  ```
**注意,break 可以break到指定label处**  

```  
  package main

  import (
      "fmt"
  )

  func main() {
      fmt.Println("1")

      Exit:
      for i := 0; i < 9; i++ {
          for j := 0; j < 9; j++ {
              if i+j > 15 {
                  fmt.Print("exit")
                  break Exit
              }
          }
      }

      fmt.Println("3")
  }

```
[参考文章:golang label breaks](http://studygolang.com/articles/2365)  
- 函数
  函数是一个非常重要的概念, 也很简单.   
  go的函数以func关键字定义, 支持不定参数和多返回值. 函数名首字符的大小写是很有讲究的:  
  小写字母开头的函数只在本包内可见，大写字母开头的函数才能被其他包使用。这个规则也适用于类型和变量的可见性。  
  ```
  package main

  import "fmt" //引入依赖包

  //...int是不定参数,实际上就是一个slice, a,b是多返回值
  //sample... //...表示是拆成一个个元素传递
  func SumAndAverage(sample ...int) (a , b float64)  {
      a , b = 0 , 0
      for _, d := range sample {
          a += float64(d)
      }
      if len(sample) == 0 {
          b = 0
      }else{
          b = a / float64(len(sample))
      }
      return a , b
  }

  func main() {
      a , b := SumAndAverage(1, 2 , 3)
      fmt.Println(a , b)
  }
  ```
  匿名函数  
  ```  
  package main

  import "fmt" //引入依赖包

  func main() {
      var myFunc func(...int)(float64, float64)= func(sample ...int) (a , b float64)  {
          a , b = 0 , 0
          for _, d := range sample {
              a += float64(d)
          }
          if len(sample) == 0 {
              b = 0
          }else{
              b = a / float64(len(sample))
          }
          return a , b
      }

      a , b := myFunc(1, 2 , 3)
      fmt.Println(a , b)
  }
  ```
- 闭包  
  1.闭包是可以包含自由（未绑定到特定对象）变量的代码块，这些变量不在这个代码块内或者任何全局上下文中定义，
  而是在定义代码块的环境中定义。  
  2.要执行的代码块（由于自由变量包含在代码块中，所以这些自由变量以及它们引用的对象没有被释放）为自由变量提供绑定的计算环境（作用域）。  
  3.闭包的实现确保只要闭包还被使用，那么被闭包引用的变量会一直存在。  
  ```
  package main

  import "fmt" //引入依赖包

  func test(i int) func()  {
      return func(){
          fmt.Println(10+i)
          fmt.Printf("%p\n",&i)
          fmt.Printf("%d\n",i)
      }

  }

  func main() {
      a := test(1);
      b := test(2)
      fmt.Println("execute func a")
      a()
      fmt.Println("execute func b")
      b()
  }
  ```
  输出结果:
  ```
  execute func a
  11
  0xc082002268
  1
  execute func b
  12
  0xc0820022a0
  2
  ```
  我们从这个结果中发现, i的地址是会变的, 因为是作为一个局部变量传进去的.  
  ```
  package main

  import "fmt" //引入依赖包

  func test(x int) func(int) int {
    return func(y int) int {
      fmt.Printf("%p\n",&x)
      return x + y
    }
  }

  func main() {
    a := test(1);
    fmt.Println(a(10))
    fmt.Println(a(20))
  }
  ```
  输出
  ```
  0xc082002268
  11
  0xc082002268
  21
  //因为x只传入了一次( test(1) ), 因此没有改变.
  ```
  ```
  package main

  import (
      "fmt"
  )

  func main() {
      var j int = 5
      a := func() func() {
          var i int = 10
          return func() {
              //i *= 3
              fmt.Printf("i, j: %d, %d\n", i, j)
          }
      }()
      a()
      j *= 2
      a()
  }

  ```