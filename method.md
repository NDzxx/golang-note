# 为类型添加方法

理念是把方法绑定到特定类型

## 对象方法

```go
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

```go
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

```
package main

import (
    "fmt"
)

//go绑定方法必须是本包内的,int不是main包内定义的.
//因此需要type一下, Integer就是本包内定义的类型
type Integer int

//为int绑定一个Print方法
//该方法用指针调用和用值去调用, 效果是一样的.
// this都是指向原来对象的指针而已.
func (this *Integer) Add() {
    fmt.Println(this)
    *this += 2
}

func main() {
    var a Integer = 10
    a.Add()
    fmt.Println(a)
    fmt.Println("---------")
    var b Integer = 10
    var c *Integer = &b
    c.Add()
    fmt.Println(c)
}
```

这说明对于对象的方法, 无论接受者是对象还是对象指针, 都没
任何问题.
但是对于接口，则完全不同

