# 多文件示例

src/test_interface.git/main.go
```
package main

import (
    task "test_interface.git/task"
)

func main() {
    do := task.NewInter(task.NewTask())
    do.OnInit()
}
```

src/test_interface.git/task/print.go
```
package task

import (
    "fmt"
)

type Stve struct {
}

func NewTask() Inter {
    return &Stve{}
}

func (self *Stve) OnInit() {
    fmt.Println("Hello wrold")
}
```
src/test_interface.git/task/interface.go
```
package task

import ()

type Inter interface {
    OnInit()
}

type App struct {
    app Inter
}

func NewInter(tmp Inter) *App {
    app1 := new(App)
    app1.app = tmp
    return app1
}

func (self *App) OnInit() {
    self.app.OnInit()
}
```
编译

go run main.go


执行结果： 
```
Hello wrold
```