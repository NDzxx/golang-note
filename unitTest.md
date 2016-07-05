# 单元测试
在需要测试的包下面创建以“_test”结尾的go文件，如*_test.go  

- 功能测试函数  
  ```
  //以Test为函数名前缀，t *testing.T为单一参数
  func TestAdd1(t *testing.T) 
  ```
- 性能测试函数