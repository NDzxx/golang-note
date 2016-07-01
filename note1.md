# 笔记1

##顺序编程
###1.变量  
go语言变量定义的关键字是var。类型放在变量名后：  
```
   var v1 int
   var v2 string
   var v3 [10]int  //数组
   var v4 []int    //切片
   var v5 struct{  //结构体
        f int
   }              
   var v6 *int  //指针
   var v7 map[string]int  //map
   var v8 func(a int) int  //函数
```
每一行不需要以分号作为结尾。
关键字还有一种是把多个变量的申明放在一起，
```
 var(
      v1 string
      v2 int
    )
```
###2.变量初始化  
```
var a int = 10  //完整定义
var a = 10  //自动推断是int型
a := 10    //自动推断是int型，申明并未该变量赋值
```
第三种初始化方式无疑是最简单的。
但是要注意的是，这里面第三种方式是和特别的，比如
```
var a int
a ：= 10
```
等价于
```
var a int
var a int
a = 10
```
这时候就会报一个重复定义的错误。
###3.变量赋值
变量赋值和我们通常的语言基本是一致的，但是多了多重赋值功能。

i，j=j,i

这就直接实现了两个变量的交换。
###4. 匿名变量
go语言的函数是多返回值的，因此可能有些值并没有被用到，这时我们就需要一个占位符去忽略这些返回值。
```
func GetName() (firstName, lastName, nickName string) {
 return "May", "Chan", "Chibi Maruko"
}
_, _, nickName := GetName()
```
###5.定义常量
通过const关键字，可以用来定义常量
```
const Pi float64 = 3.1415926
const zero = 0.0 //自动推断类型
const (             //多定义
  size int64 = 10
  hello = -1
)
const u , v float32 = 0.0 , 3.0 //多重赋值
const a , b , c = 1 , 2 , “hello” //自动推断类型
```
常量的定义也可以跟一个表达式, 但是这个表达式应该是编译的时候就可以求值的.
```
const mask = 1 << 3 //正常
const Home = os.GetEnv("HOME") //错误,运行时才能确定
```
