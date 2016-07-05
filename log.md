# log类的使用
```
package main

import (
	"fmt"
	"log"
	"os"
	//"io/ioutil"
	//"net/http"
	"runtime"
	"path"
	"strings"
)

// GetCurFilename
// Get current file name, without suffix
func GetCurFilename() string {
	_, fulleFilename, _, _ := runtime.Caller(0)
	//fmt.Println(fulleFilename)
	var filenameWithSuffix string
	filenameWithSuffix = path.Base(fulleFilename)
	//fmt.Println("filenameWithSuffix=", filenameWithSuffix)
	var fileSuffix string
	fileSuffix = path.Ext(filenameWithSuffix)
	//fmt.Println("fileSuffix=", fileSuffix)

	var filenameOnly string
	filenameOnly = strings.TrimSuffix(filenameWithSuffix, fileSuffix)
	//fmt.Println("filenameOnly=", filenameOnly)

	return filenameOnly
}

func main() {
	fmt.Printf("this is EmulateLoginBaidu.go\n")

	var filenameOnly string
	filenameOnly = GetCurFilename()
	fmt.Println("filenameOnly=", filenameOnly)

	var logFilename string =  filenameOnly + ".log";
	fmt.Println("logFilename=", logFilename)
	logFile, err := os.OpenFile(logFilename, os.O_RDWR | os.O_CREATE, 0777)
	if err != nil {
		fmt.Printf("open file error=%s\r\n", err.Error())
		os.Exit(-1)
	}

	defer logFile.Close()
	logger:=log.New(logFile,"\r\n", log.Ldate | log.Ltime | log.Llongfile)
	//logger:=log.New(logFile,"\r\n", log.Ldate | log.Ltime | log.Lshortfile)
	logger.Println("normal log 1")
	logger.Println("normal log 2")
}

```
