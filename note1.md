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
