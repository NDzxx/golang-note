# 接口
go的借口是非侵入式的, 只要实现了接口定义的方法, 就等于实现了该接口。  
换句话说, 接口的实现和定义式可以分离不相关的。  
例子：  
```
package main

import(
  "fmt"
)

type Integer int

func (a Integer) Less(b Integer) bool {
  return a < b
}

func (a *Integer) Add(b Integer) {
  *a += b
}

//定义接口
type LessAdder interface {
  Less(b Integer) bool //函数声明
  Add(b Integer) //函数声明
}

func main() {
  var a Integer = 10
  var b LessAdder = &a //Add接收者是个对象指针，原因见接口大坑
  fmt.Println(b.Less(5))
  b.Add(20)
  fmt.Println(a)
}
```
只要两个接口拥
有相同的方法列表（次序不同不要紧），那么它们就是等同的，可以相互赋值。

接口赋值并不要求两个接口必须等价。如果接口A的方法列表是接口B的方法列表的子集，
那么接口B可以赋值给接口A。  
几个接口也可以组合出一个接口.
```
type ReadWriter interface {
 Reader
 Writer
}
```
等价于:
```
type ReadWriter interface {
 Read(p []byte) (n int, err error)
 Write(p []byte) (n int, err error)
}
```
##接口的大坑


**先上结论：
这说明对于对象的方法, 无论接受者是对象还是对象指针, 都没
任何问题.   
但是如果是接口,如果接口中存在某个方法,绑定的接收者是对象指针,那么这个接口
也只能被该对象指针赋值.**

看例子1
```
package main

import(
	"fmt"
)
//定义接口Speaker和Learner
type Speaker interface{
	SayHi()
}

type Learner interface{
	SayHi()
	Study()
}

//定义对象People、Teacher和Student
type People struct {
	Name string
}

type Teacher struct{
	People
	Department string
}

type Student struct{
	People
	School string
}

//对象方法实现
func (p *People) SayHi() {
	fmt.Printf("Hi, I'm %s. Nice to meet you!\n",p.Name)
}

func (t *Teacher) SayHi(){
	fmt.Printf("Hi, my name is %s. I'm working in %s .\n", t.Name, t.Department)
}

func (s *Student) SayHi() {
	fmt.Printf("Hi, my name is %s. I'm studying in %s.\n", s.Name, s.School)
}

func (s *Student) Study() {
	fmt.Printf("I'm learning Golang in %s.\n", s.School)
}


func main() {

	//初始化对象
	people := People{"张三"}
	//  teacher := &Teacher{People{"郑智"}, "Computer Science"}
	//  student := &Student{People{"李明"}, "Yale University"}

	var speaker Speaker   //定义Speaker接口类型的变量

	speaker = people
	speaker.SayHi()
}
```
结果：
```
# command-line-arguments
src\goMethod_1.go:58: cannot use people (type People) as type Speaker in assignment:
	People does not implement Speaker (SayHi method has pointer receiver)
```
修改一下
```
//speaker = people
speaker = &people
speaker.SayHi()
```
结果：
```
Hi, I'm 张三. Nice to meet you!
```

或者把传指针改成传值
```
package main

import(
	"fmt"
)

//定义对象People、Teacher和Student
type People struct {
	Name string
}

type Teacher struct{
	People
	Department string
}

type Student struct{
	People
	School string
}

//对象方法实现
func (p People) SayHi() {
	fmt.Printf("Hi, I'm %s. Nice to meet you!\n",p.Name)
}

func (t Teacher) SayHi(){
	fmt.Printf("Hi, my name is %s. I'm working in %s .\n", t.Name, t.Department)
}

func (s Student) SayHi() {
	fmt.Printf("Hi, my name is %s. I'm studying in %s.\n", s.Name, s.School)
}

func (s Student) Study() {
	fmt.Printf("I'm learning Golang in %s.\n", s.School)
}

//定义接口Speaker和Learner
type Speaker interface{
	SayHi()
}

type Learner interface{
	SayHi()
	Study()
}

func main() {

	//初始化对象
	people := People{"张三"}
	var speaker Speaker   //定义Speaker接口类型的变量

	speaker = people
	speaker.SayHi()
}
```
结果:  
```
Hi, I'm 张三. Nice to meet you!
```
##Any类型
由于Go语言中任何对象实例都满足接口interface{}，所以interface{}看起来是任何对象的Any类型
```
var v1 interface{} = 1
var v2 interface{} = "abc"
var v3 interface{} = &v2
var v4 interface{} = struct{ X int }{1}
var v5 interface{} = &struct{ X int }{1}
func Printf(fmt string, args ...interface{})
func Println(args ...interface{})
```
##接口转换和类型查询
- 接口转换  
 
```
package main

import(
  "fmt"
)

type Integer int

func (a Integer) Less(b Integer) bool {
  return a < b
}

func (a *Integer) Add(b Integer) {
  *a += b
}

//定义接口
type LessAdder interface {
  Less(b Integer) bool //函数声明
  Add(b Integer) //函数声明
}

//定义接口
type Adder interface {
  Add(b Integer) //函数声明
}

func main() {
  var a Integer = 10
  var b LessAdder = &a //道理我们前面提到过了,Add接收者是个对象指针
  if  c , ok = b.(Adder); ok{
    c.Add(10)
    fmt.Println(a)
    //fmt.Println(c.Less(100)) 
    //报错,c.Less undefined (type Adder has no field or method Less)
  }
}
```  
- 类型查询  

```
package main

import(
  "fmt"
)


func main() {
  b := "a"
  var a interface{} = b//利用any类型进行查询
  switch a.(type) {
  case int:
    fmt.Println("int")
  case float32:
    fmt.Println("float32")
  case int32:
    fmt.Println("int32")
  case float64:
    fmt.Println("float64")
  case bool:
    fmt.Println("bool")
  case string:
    fmt.Println("string")
  default:
    fmt.Println("ok")

  }
}
```
 