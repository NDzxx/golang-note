#redis golang驱动测试

golang官方推荐的redis驱动有两个，redigo和radix

现在测试一下二者的执行效率
```
package main

import (

 "testing"

 "github.com/fzzy/radix/redis"

)

var radixRedisClient *redis.Client

func TestRadixConnect(t *testing.T) {

 var err error

 radixRedisClient, err = redis.Dial("tcp", "127.0.0.1:6379")

 if err != nil {

 t.Fatalf("Connect failed: %v", err)

 }

 reply := radixRedisClient.Cmd("PING")

 if reply.Err != nil {

 t.Fatalf("Command failed: %v", err)

 }

 s, replyErr := reply.Str()

 if s != "PONG" {

 t.Fatalf("ping failed. Got %s, err:%s,expected PONG.", s, replyErr)

 }

}

func BenchmarkRadixPing(b *testing.B) {

 radixRedisClient.Cmd("DEL", "hello")

 for i := 0; i < b.N; i++ {

 reply := radixRedisClient.Cmd("PING")

 if reply.Err != nil {

 b.Fatalf(reply.Err.Error())

 break

 }

 }

}

func BenchmarkRadixSet(b *testing.B) {

 var err error

 for i := 0; i < b.N; i++ {

 reply := radixRedisClient.Cmd("SET", "hello", 1)

 if reply.Err != nil {

 b.Fatalf(err.Error())

 break

 }

 }

}

func BenchmarkRadixGet(b *testing.B) {

 var err error

 for i := 0; i < b.N; i++ {

 reply := radixRedisClient.Cmd("GET", "hello")

 if reply.Err != nil {

 b.Fatalf(err.Error())

 break

 }

 }

}

func BenchmarkRadixRedisIncr(b *testing.B) {

 var err error

 for i := 0; i < b.N; i++ {

 reply := radixRedisClient.Cmd("INCR", "hello")

 if reply.Err != nil {

 b.Fatalf(err.Error())

 break

 }

 }

}

func BenchmarkRadixRedisLPush(b *testing.B) {

 var err error

 radixRedisClient.Cmd("DEL", "hello")

 for i := 0; i < b.N; i++ {

 reply := radixRedisClient.Cmd("LPUSH", "hello", i)

 if reply.Err != nil {

 b.Fatalf(err.Error())

 break

 }

 }

}

func BenchmarkRadixRedisLRange10(b *testing.B) {

 var err error

 for i := 0; i < b.N; i++ {

 reply := radixRedisClient.Cmd("LRANGE", "hello", 0, 10)

 if reply.Err != nil {

 b.Fatalf(err.Error())

 break

 }

 }

}

func BenchmarkRadixRedisLRange100(b *testing.B) {

 var err error

 for i := 0; i < b.N; i++ {

 reply := radixRedisClient.Cmd("LRANGE", "hello", 0, 100)

 if reply.Err != nil {

 b.Fatalf(err.Error())

 break

 }

 }

}
```


