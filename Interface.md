# 接口
go的借口是非侵入式的, 只要实现了接口定义的方法, 就等于实现了该接口。  
换句话说, 接口的实现和定义式可以分离不相关的。  



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