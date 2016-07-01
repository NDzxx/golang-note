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