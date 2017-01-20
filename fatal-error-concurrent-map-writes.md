#concurrent map writes

 golang map数据结构不能并发读写导致问题
 
 问题由来

今天，我在编码并发测试过程中遇到一个问题直接致死整个进程。我们知道golang 中只要有一个goroutine发生panic整个进程都挂了。当时一脸萌比。开始检查堆栈信息。

我查阅了相关问题解决方案。大致就是多线程操作map数据结构一定要加锁。否则肯定要出现这个错误。

解决方案
 
 加上读写锁
 
 ```go
 type DbWebAdmin struct {
	mapDbWebAdmin map[string]*ServerCfg //key:Ip
	mapDbUser     map[string]*UserCfg   //key:username
	mapLoginUser  map[string]*UserCfg   //key:ip
	sliceAnnounce []*AnnounceData
	Lock          sync.RWMutex
}

//写的加写锁
func (self *DbWebAdmin) InsertDefaultAdmin(name string, passwd string, bindip string) (bool, error) {
	self.Lock.Lock()
	defer self.Lock.Unlock()
	dbObj := orm.NewOrm()
	...
}

//读的加读锁
func (self *DbWebAdmin) LoadAllUserInfo() {
	self.Lock.Lock()
	defer self.Lock.Unlock()
	dbObj := orm.NewOrm()
	dbObj.Using(common.ALIAS_WEBADMIN)
	...
}
```
