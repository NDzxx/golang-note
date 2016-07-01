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
      fmt.Println("sa len:",len(sa))
      fmt.Println("sa cap:",cap(sa))
      fmt.Println("sa done")
      //从make创建,make第2个参数是长度，第3个参数是开辟的空间上限
      var mySlice2 []int = make([]int, 5, 10)
      /*错误例子，这样会超出长度
      for i := 0; i < cap(mySlice2); i++{
          mySlice2[i] = i
      }*/
      //可以使用append来增加数组长度或者赋值给别的数组,可突破设置的空间上限
      //mySlice2 = append(mySlice2,0,1,2,3,4,5,6,7,8,9,10)
      for _, e := range mySlice2 {
          fmt.Println(e)
      }
      fmt.Println("mySlice2 len:",len(mySlice2))
      fmt.Println("mySlice2 cap:",cap(mySlice2))
      fmt.Println("mySlice2 done")
      //赋值
      var mySlice3 []int = []int{1,2,3}
      fmt.Println("mySlice3 len:",len(mySlice3))
      //append注意点，如果添加给mySlice3,
      //这样会抹掉mySlice3的值但是myslice2的值没有变化
      mySlice3 = append(mySlice2,0,1,2,3,4,5,6,7,8,9,10)
      fmt.Println("mySlice3 len:",len(mySlice3))
      fmt.Println("mySlice3:",mySlice3)
      fmt.Println("mySlice3 done")
      fmt.Println("mySlice2 len:",len(mySlice2))
      fmt.Println("mySlice2:",mySlice2)
  }
  ```
- slice和数组传参的比较  

  ```  
  package main

  import "fmt" //引入依赖包

  func testArr(a [10]int)  {
      a[0] = 10
  }

  func testSlice(a []int)  {
      a[0] = 10
  }

  func main() {
      //数组是按照值来传递.
      var myArray [10]int = [10]int{1,2,3,4,5,6,7,8,9,10}
      fmt.Println("arr before test func",myArray)
      testArr(myArray)
      fmt.Println("arr after test func",myArray)
      //slice是引用类型!!!
      var mySlice []int = []int{1,2,3,4,5,6,7,8,9,10}
      fmt.Println("slice before test func",mySlice)
      testSlice(mySlice)
      fmt.Println("slice after test func",mySlice)
  }
  ```
- append  
  ```
  mySlice = append(mySlice, 1, 2, 3)
  mySlice = append(mySlice, mySlice2...)//append可以添加slice到slice
  ```
- map  
  用法见例子
  ```  
  package main

  import "fmt" //引入依赖包

  //定义一个Person的结构体
  type Person struct{
    name string
    age int
  }

  func main() {
    var dic map[string]Person = make(map[string]Person , 100) //初始化map
    //插入
    dic["1234"] = Person{name:"lilei",age:100}
    dic["12345"] = Person{name:"hanmeimei",age:20}
    dic["123456"] = Person{name:"dagong",age:30}
    fmt.Println(dic)
    //删除dagong
    delete(dic,"123456")
    fmt.Println(dic)
    //查找某个key
    value,ok := dic["123456"]
    if ok {
        fmt.Println(value)
    }
    value,ok = dic["1234"]
    if ok {
        fmt.Println(value)
    }

    for k,v := range dic {
      fmt.Println(k + ":" + v.name)
    }
  }
  ```