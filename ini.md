# ini

使用无闻的ini库：[https:\/\/github.com\/go-ini\/ini\/](https://github.com/go-ini/ini/)

```golang
package main

import (

"fmt"

"github.com/go-ini/ini"

)

type Http struct {

Port string `ini:"port"`

Mode string `ini:"mode"`

}

type DataBase struct {

Ip string `ini:"ip"`

User string `ini:"user"`

Password string `ini:"password"`

Dbname string `ini:"dbname"`

Port string `ini:"port"`

}

type iniCfg struct {

Http

DataBase

}

func main() {

var iniPath string = "./conf/config.ini"

cfg := new(iniCfg)

err := ini.MapTo(cfg, iniPath)

if err == nil{

fmt.Println(cfg)

}

}



```