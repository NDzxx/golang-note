#GIn使用
[readMe](https://github.com/gin-gonic/gin)

[框架简介：中文readme](http://blog.csdn.net/moxiaomomo/article/details/51153779)
[安装](http://qq466862016.iteye.com/blog/2276960)

##载入读取js+gtml
```golang
package main

import "github.com/gin-gonic/gin"
import "net/http"
func main() {
     router := gin.Default()
    router.LoadHTMLGlob("views/*")
    router.Static("/static", "./static")
    router.StaticFile("/favicon.ico", "./static/favicon.ico")
    router.GET("/index", func(c *gin.Context) {
        c.HTML(http.StatusOK, "index.html", gin.H{
            "title": "Main website",
        })
    })
    router.GET("/ping", func(c *gin.Context) {
        c.String(http.StatusOK, "pong")
    })
    router.Run(":6354")
}

```

```
//err := errors.New("您输入不是双数")
    //mdlog.Error(err)


package main

import (
	"fmt""net/url""net/http""io/ioutil""log""bytes""encoding/json"
)

type Server struct {
	ServerName string
	ServerIP   string
}

type Serverslice struct {
	Servers []Server
	ServersID  string
}


func main() {

	var s Serverslice

	var newServer Server;
	newServer.ServerName = "Guangzhou_VPN";
	newServer.ServerIP = "127.0.0.1"       
	s.Servers = append(s.Servers, newServer)

	s.Servers = append(s.Servers, Server{ServerName: "Shanghai_VPN", ServerIP: "127.0.0.2"})
	s.Servers = append(s.Servers, Server{ServerName: "Beijing_VPN", ServerIP: "127.0.0.3"})
	
	s.ServersID = "team1"

	b, err := json.Marshal(s)
	if err != nil {
		fmt.Println("json err:", err)
	}

	body := bytes.NewBuffer([]byte(b))
	res,err := http.Post("http://localhost:9001/xiaoyue", "application/json;charset=utf-8", body)
	if err != nil {
		log.Fatal(err)
		return
	}
	result, err := ioutil.ReadAll(res.Body)
	res.Body.Close()
	if err != nil {
		log.Fatal(err)
		return
	}
	fmt.Printf("%s", result)
}

来源： http://www.tuicool.com/articles/uQJf2a
```

```
package main

import (
	"fmt""net/http""strings""html""io/ioutil""encoding/json"
)

type Server struct {
	ServerName string
	ServerIP   string
}

type Serverslice struct {
	Servers []Server
	ServersID  string
}

func main() {
	http.HandleFunc("/", handler) 
	http.ListenAndServe(":9001", nil)
}

func handler(w http.ResponseWriter, r *http.Request) { 
	r.ParseForm() //解析参数，默认是不会解析的 
	fmt.Fprintf(w, "Hi, I love you %s", html.EscapeString(r.URL.Path[1:]))
	if r.Method == "GET" {
		fmt.Println("method:", r.Method) //获取请求的方法 

		fmt.Println("username", r.Form["username"]) 
		fmt.Println("password", r.Form["password"]) 

		for k, v := range r.Form {
			fmt.Print("key:", k, "; ")
			fmt.Println("val:", strings.Join(v, ""))
		}
	} else if r.Method == "POST" {
		result, _:= ioutil.ReadAll(r.Body)
		r.Body.Close()
		fmt.Printf("%s\n", result)

		//未知类型的推荐处理方法var f interface{}
		json.Unmarshal(result, &f) 
		m := f.(map[string]interface{})
		for k, v := range m {
			switch vv := v.(type) {
				case string:
					fmt.Println(k, "is string", vv)
				case int:
					fmt.Println(k, "is int", vv)
				case float64:
					fmt.Println(k,"is float64",vv)
				case []interface{}:
					fmt.Println(k, "is an array:")
					for i, u := range vv {
						fmt.Println(i, u)
					}
				default:
					fmt.Println(k, "is of a type I don't know how to handle") 
			 }
		  }

		 //结构已知，解析到结构体var s Serverslice;
		 json.Unmarshal([]byte(result), &s)

		 fmt.Println(s.ServersID);
  
		 for i:=0; i<len(s.Servers); i++ {
			 fmt.Println(s.Servers[i].ServerName)
			 fmt.Println(s.Servers[i].ServerIP)
		 }
	} 
}

```