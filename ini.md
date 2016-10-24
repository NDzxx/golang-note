# ini

使用无闻的ini库：[https:\/\/github.com\/go-ini\/ini\/](https://github.com/go-ini/ini/)

    package main

    import (
        "fmt"

        "github.com/go-ini/ini"
    )

    type Database struct {
        Ip       string `ini:"ip"`
        User     string `ini:"user"`
        Password string `ini:"password"`
        Dbname   string `ini:"dbname"`
        Port     string `ini:"port"`
    }

    type BoxAccountDatabase struct {
        Ip       string `ini:"ip"`
        User     string `ini:"user"`
        Password string `ini:"password"`
        Dbname   string `ini:"dbname"`
        Port     string `ini:"port"`
    }

    type Logger struct {
        Level int `ini:"level"`
    }

    type iniCfg struct {
        Database
        BoxAccountDatabase
        Logger
    }

    func main() {
        var iniPath string = "E:/Go_src/ini/iniFile/config.ini"
        cfg, err := ini.Load(iniPath)
        if err == nil {
            cfg.BlockMode = false
            pini := new(iniCfg)
            err_w := cfg.MapTo(pini)
            if err_w == nil {
                fmt.Println(pini)
            }
        }

        //fmt.Println(cfg.Section("ServerConfig").Key("ip").String())

    }

    /*
     func main() { 

```
    var iniPath string = "./conf/config.ini"
```

```
    cfg := new(iniCfg)
```

```
    err := ini.MapTo(cfg, iniPath)
```

```
    if err == nil{
```

```
        fmt.Println(cfg)
```

```
    }
```

```
   
```

```

```

```
}
```



\*\/


