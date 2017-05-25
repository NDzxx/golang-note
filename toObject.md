# 面向对象编程
##概念
```
b = a
b.Mofify()
```
如果b改变, a不发生改变, 就是值语义. 如果b改变, a也发生改变, 就是引用语义.
- 值语义   
  基本类型: byte, int, float32, float64, string  
  复合类型: struct, array, pointer  
- 引用语义  
  slice, map, channel, interface.  
  
##结构体  
```
  package main

  import (
      "fmt"
  )

  //申明一个结构体
  type Person struct {
      Name string
      Age  int
  }

  func main() {

      //结构体的初始化方式
      //1. 直接赋值
      var p Person
      p.Name = "dingding"
      p.Age = 10
      fmt.Println(p)

      //2.顺序赋值
      p1 := Person{"dingding", 10}
      fmt.Println(p1)

      //3. key value 赋值
      p2 := Person{Name: "dingding", Age: 10}
      fmt.Println(p2)

      //4.指针赋值
      p3 := &Person{Name: "dingding", Age: 10}
      fmt.Println(p3)
      p4 := new(Person)
      fmt.Println(p4)

      fmt.Println("---------------------------")

      a := p
      a.Name = "dongdong"
      b := p3
      b.Name = "dongdong"
      fmt.Println("a Name",a)
      fmt.Println("p Name",p)
      //传值才改变了b的值，struct是值引用
      fmt.Println("b Name",b)
      fmt.Println("p3 Name",p3)

  }
```
###结构体的组合  
```
package main

import (
 "fmt"
)

//申明一个结构体
type Human struct{
  Name string
  Age int
  Phone string
}

//再申明一个结构体
type Employee struct {
    Person Human  // 匿名字段Human
    Speciality string
    Phone string  // 雇员的phone字段
}

func main() {
  e := Employee{
    Person:Human{
      Name:"dingding",
      Age:11,
      Phone:"6666666",
    },
    Speciality:"aaa",
    Phone:"77777777",
  }
  fmt.Println(e.Phone)

}
```
这段代码看上去非常ok, 但是如果我们稍微改一下代码呢?   
比如把Person改成匿名结构, 会有些好玩的现象.
```
package main

import (
 "fmt"
)

//申明一个结构体
type Human struct{
  Name string
  Age int
  Phone string
}

//再申明一个结构体
type Employee struct {
    Human  // 匿名字段Human
    Speciality string
    //Phone string  // 雇员的phone字段
}

func main() {
  e := Employee{
    Human{"dingding",11,"6666666"},
    "aaa",
    //Phone:"77777777",
  }
  fmt.Println(e.Phone)

}
```
此时输出的是6666666, 因为相当于它把Human的字段Phone继承下来了.  
如果Employee里也定义Phone字段呢?  
```
package main

import (
 "fmt"
)

//申明一个结构体
type Human struct{
  Name string
  Age int
  Phone string
}

//再申明一个结构体
type Employee struct {
    Human  // 匿名字段Human
    Speciality string
    Phone string  // 雇员的phone字段
}

func main() {
  e := Employee{
    Human{"dingding",11,"6666666"},
    "aaa",
    "77777777",
  }
  fmt.Println(e.Phone)

}
```
此时输出的时77777777, 因为相当于是覆盖. 那么怎么输出6666666呢?
```
package main

import (
 "fmt"
)

//申明一个结构体
type Human struct{
  Name string
  Age int
  Phone string
}

//再申明一个结构体
type Employee struct {
    Human  // 匿名字段Human
    Speciality string
    Phone string  // 雇员的phone字段
}

func main() {
  e := Employee{
    Human{"dingding",11,"6666666"},
    "aaa",
    "77777777",
  }
  fmt.Println(e.Phone)
  fmt.Println(e.Human.Phone)
}
```
输出结果:  
```
77777777
6666666

```
所以, 匿名结构体的组合相当于有继承的功能.