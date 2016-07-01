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

