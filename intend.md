# 类似继承的处理方式

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
```
package main

import(
  "fmt"
)

//father
type Father struct{
}

func (f *Father)Say()  {
  fmt.Println("father")
}


//mother
type Mother struct{
}

func (f *Mother)Say()  {
  fmt.Println("mother")
}

//sun
type Sun struct{
  Father
  Mother
}

func main() {
  var s *Sun = new(Sun)
  s.Say()//二义性
}
```
结果：
```
# command-line-arguments
src\moreIntend.go:32: ambiguous selector s.Say
```
修改方式：
```
func main() {
  var s *Sun = new(Sun)
  s.Father.Say()
  s.Mother.Say()
}
```
##另一种用法
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
  *Base  //base是个指针
  Name string
}

//重写Say方法
func (f *Foo) Say()  {
  f.Base.Say()
  fmt.Println(f.Name)
}

func main() {
  var f *Foo = &Foo{&Base{"father"},"sun"}
  f.Ok()
  f.Say()
}
```
这里Foo的Base是一个指针类型, 因此我们传入的也必须是个指针, 我们可以看到这种用法.