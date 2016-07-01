# 为类型添加方法
```
package main

import (
 "fmt"
)

//go绑定方法必须是本包内的,int不是main包内定义的.
//因此需要type一下, Integer就是本包内定义的类型
type Integer int

//为int绑定一个Print方法
func (i Integer) Println() {
  fmt.Println(i)
}

func main() {
  var a Integer = 10
  a.Println()
}
```
结果输出10, 如果是如下呢?
```
package main

import (
 "fmt"
)

//go绑定方法必须是本包内的,int不是main包内定义的.
//因此需要type一下, Integer就是本包内定义的类型
type Integer int

//为int绑定一个Print方法
//注意:这里的i并不是引用类型,因此对i的修改不会反应到a上去.
func (i Integer) Println() {
  fmt.Println(i)
}

func main() {
  a := 10
  a.Println()
}
```
结果：  
```
# command-line-arguments
.\goSrc.go:18: a.Println undefined (type int has no field or method Println)
```
因为a := 10, go会把a推断为int, 但int并没绑上Println方法.
