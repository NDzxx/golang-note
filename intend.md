# 继承

go的继承非常简陋, 就是一个匿名结构组合的问题. 不废话,直接上代码.
```
package main

import(
	"fmt"
)

//基类
type Base struct{
	Name string
}

//绑定Say方法
func (b *Base) Say()  {
	fmt.Println(b.Name)
}

//绑定ok方法
func (b *Base) Ok()  {
	fmt.Println("ok")
}

//Foo有个匿名结构Base
type Foo struct{
	Base
	Name string
}

//重写Say方法
func (f *Foo) Say()  {
	f.Base.Say()
	fmt.Println(f.Name)
}

func main() {
	var f *Foo = &Foo{Base{"father"},"sun"}
	f.Ok()
	f.Say()
}
```
结果:
```
ok
father
sun
```

##多继承二义性
