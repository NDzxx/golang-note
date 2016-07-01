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
