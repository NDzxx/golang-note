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
  
  