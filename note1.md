# 笔记1

##顺序编程
1.变量  
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
