##直接分割beego的数据库框架orm可以使用

```golang
 package main import ( "fmt" "orm" _ "github.com/go-sql-driver/mysql" ) type ZxxTable struct { Id int Name string `orm:"size(100)"` Ip string `orm:"size(20)"` } func init() { orm.RegisterModel(new(ZxxTable)) orm.RegisterDataBase("default", "mysql", "root:root@/logdb?charset=latin1", 30) orm.RunSyncdb("default", false, true) // true 改成false，如果表存在则会给出提示，如果改成false则不会提示 ， 这句话没有会报主键不存在的错误 } func main() { o := orm.NewOrm() testTable := ZxxTable{Name: "Musikar", Ip: "127.0.0.1"} //增 id, err := o.Insert(&testTable) fmt.Println(id, err, testTable.Id) //改 testTable.Name = "Hacker" num, err := o.Update(&testTable) fmt.Println(num, err) //查 u := ZxxTable{Id: testTable.Id} err = o.Read(&u) fmt.Println(u) //删 //num, err = o.Delete(&u) //fmt.Println(num, err) } 
```
  