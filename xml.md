# xml处理

##函数
func Marshal(v interface{}) ([]byte, error)  
func Unmarshal(data []byte, v interface{}) error  
上面两个函数的文档（注释）很长。从文档知道，Marshal是将v代表的数据转为XML格式（生成XML）；而Unmarshal刚好相反，是解析XML，同时将解析的结果保存在v中。  

func MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)  
这个函数和Marshal的不同是，每个XML元素会增加前缀和缩进  
例子：  
```
<?xml version="1.0" encoding="utf-8"?>
<actionList>
 <act actName="盒子测试正常1">
    <item title="登陆通行证"><!-- MsgPassportLoginC2S-->
      <param name="passportName" type="string" num="15200000001" />
    </item>
    <item title="查询通行证"><!-- MsgReqPassportC2S-->
    </item>
   <item title="核查通行证验证码"><!-- MsgCheckPassportVeriCodeC2S-->
      <param name="passportName" type="string" num="15200000001" />
      <param name="veriCode" type="string" num="123456" />
    </item>
<item title="查询魔域游戏锁"><!--   MsgQueryLockStatusC2S对应发送SetVipLockAccount-->
  <param name="passportName" type="string" num="15200000001" />
</item>
<item title="锁定魔域游戏锁"><!--   MsgLockGameC2S对应发送SetVipLockAccount-->
  <param name="passportName" type="string" num="15200000001" />
</item>
<item title="解锁魔域游戏锁"><!--   MsgLockGameC2S对应发送SetVipLockAccount-->
  <param name="passportName" type="string" num="15200000001" />
</item>
       <item title="登出盒子">
      <param name="passportName" type="string" num="15200000001" />
      <param name="veriCode" type="string" num="123456" />
    </item>
  </act>
</actionList>
```

```
package main

import (
	"encoding/xml"
	"io/ioutil"
	"log"
)

type ActResult struct {
	XMLName xml.Name `xml:"actionList"`
	Act []Act `xml:"act"`
}

type Act struct {
	ActName      string   `xml:"actName,attr"`
	Item []Item `xml:"item"`
}

type Item struct {
	Title      string   `xml:"title,attr"`
	Param []Param `xml:"param"`
}

type Param struct {
	Name      string   `xml:"name,attr"`
	Type      string   `xml:"type,attr"`
	Num       string   `xml:"num,attr"`
}

func main() {
	content, err := ioutil.ReadFile("E:/Go_src/myXMl/xml/行为管理.xml")
	if err != nil {
		log.Fatal(err)
	}
	var actResult ActResult
	err = xml.Unmarshal(content, &actResult)
	if err != nil {
		log.Fatal(err)
	}
	log.Println(actResult)
	log.Println(actResult.Act[0].Item[0].Title)
}

```