# xml处理

##函数
func Marshal(v interface{}) ([]byte, error)  
func Unmarshal(data []byte, v interface{}) error  
上面两个函数的文档（注释）很长。从文档知道，Marshal是将v代表的数据转为XML格式（生成XML）；而Unmarshal刚好相反，是解析XML，同时将解析的结果保存在v中。  

func MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)  
这个函数和Marshal的不同是，每个XML元素会增加前缀和缩进  
