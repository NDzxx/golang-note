# 数组和slice
- 数组
  ```
  var v1 [32]byte
  var v2 [2*N] struct { x, y int32 }
  var v3 [1000]*float64
  var v4 [3][5]int
  var v5 [2][2][2]float64
  ```
  数组的遍历和字符串一样,这里不再重复.
  数组是值类型,在赋值时会拷贝一份.
- 数组切片（slice，类似C++ vector）
  - 切片的创建
  切片有两种创建方式, 一种是基于数组创建, 另一种是用make创建.  
  
  ```
  package main

  import "fmt" //引入依赖包

  func main() {
    //从数组创建
    var myArray [10]int = [10]int{1,2,3,4,5,6,7,8,9,10}
    var sa []int = myArray[5:]
    for _, e := range sa {
        fmt.Println(e)
    }
    fmt.Println(len(sa))
    fmt.Println(cap(sa))//cap()函数返回的是数组切片分配的空间大小。
    //从make创建
    var mySlice2 []int = make([]int, 5, 10)
    for _, e := range mySlice2 {
        fmt.Println(e)
    }
    fmt.Println(len(mySlice2))
    fmt.Println(cap(mySlice2))
    //赋值
    var mySlice3 []int = []int{1,2,3}
    for _, e := range mySlice2 {
        fmt.Println(e)
    }
  }
  ```
