# redis golang驱动测试

golang官方推荐的redis驱动有两个，redigo和radix

现在测试一下二者的执行效率 radix

```go
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

再看redigo测试代码

```go
package main



import (

"testing"
"github.com/garyburd/redigo/redis"

)



var garyburdRedigoClient redis.Conn



func TestGaryburdRedigoConnect(t *testing.T) {

var s interface{}

var err error



garyburdRedigoClient, err = redis.Dial("tcp", "127.0.0.1:6379")



if err != nil {

t.Fatalf("Connect failed: %v", err)

}



s, err = garyburdRedigoClient.Do("PING")



if err != nil {

t.Fatalf("Command failed: %v", err)

}



if s.(string) != "PONG" {

t.Fatalf("Failed")

}

}



func BenchmarkGaryburdRedigoPing(b *testing.B) {

var err error

garyburdRedigoClient.Do("DEL", "hello")

for i := 0; i < b.N; i++ {

_, err = garyburdRedigoClient.Do("PING")

if err != nil {

b.Fatalf(err.Error())

break

}

}

}



func BenchmarkGaryburdRedigoSet(b *testing.B) {

var err error

for i := 0; i < b.N; i++ {

_, err = garyburdRedigoClient.Do("SET", "hello", 1)

if err != nil {

b.Fatalf(err.Error())

break

}

}

}



func BenchmarkGaryburdRedigoGet(b *testing.B) {

var err error

for i := 0; i < b.N; i++ {

_, err = garyburdRedigoClient.Do("GET", "hello")

if err != nil {

b.Fatalf(err.Error())

break

}

}

}



func BenchmarkGaryburdRedigoIncr(b *testing.B) {

var err error

for i := 0; i < b.N; i++ {

_, err = garyburdRedigoClient.Do("INCR", "hello")

if err != nil {

b.Fatalf(err.Error())

break

}

}

}



func BenchmarkGaryburdRedigoLPush(b *testing.B) {

var err error

garyburdRedigoClient.Do("DEL", "hello")

for i := 0; i < b.N; i++ {

_, err = garyburdRedigoClient.Do("LPUSH", "hello", i)

if err != nil {

b.Fatalf(err.Error())

break

}

}

}



func BenchmarkGaryburdRedigoLRange10(b *testing.B) {

var err error

for i := 0; i < b.N; i++ {

_, err = garyburdRedigoClient.Do("LRANGE", "hello", 0, 10)

if err != nil {

b.Fatalf(err.Error())

break

}

}

}



func BenchmarkGaryburdRedigoLRange100(b *testing.B) {

var err error

for i := 0; i < b.N; i++ {

_, err = garyburdRedigoClient.Do("LRANGE", "hello", 0, 100)

if err != nil {

b.Fatalf(err.Error())

break

}

}

}

```
测试结果：
win

PASS

BenchmarkGaryburdRedigoPing-8 50000 29163 ns/op

BenchmarkGaryburdRedigoSet-8 50000 33424 ns/op

BenchmarkGaryburdRedigoGet-8 50000 31694 ns/op

BenchmarkGaryburdRedigoIncr-8 50000 32754 ns/op

BenchmarkGaryburdRedigoLPush-8 50000 32374 ns/op

BenchmarkGaryburdRedigoLRange10-8 30000 40621 ns/op

BenchmarkGaryburdRedigoLRange100-8 20000 83660 ns/op

BenchmarkRadixPing-8 50000 31594 ns/op

BenchmarkRadixSet-8 50000 34164 ns/op

BenchmarkRadixGet-8 50000 33064 ns/op

BenchmarkRadixRedisIncr-8 50000 34044 ns/op

BenchmarkRadixRedisLPush-8 50000 33714 ns/op

BenchmarkRadixRedisLRange10-8 30000 45605 ns/op

BenchmarkRadixRedisLRange100-8 10000 123662 ns/op

Linux

PASS

BenchmarkGaryburdRedigoPing-4 50000 79361 ns/op

BenchmarkGaryburdRedigoSet-4 30000 81639 ns/op

BenchmarkGaryburdRedigoGet-4 30000 81025 ns/op

BenchmarkGaryburdRedigoIncr-4 30000 81883 ns/op

BenchmarkGaryburdRedigoLPush-4 30000 82164 ns/op

BenchmarkGaryburdRedigoLRange10-4 30000 86393 ns/op

BenchmarkGaryburdRedigoLRange100-4 30000 110563 ns/op

BenchmarkRadixPing-4 50000 84643 ns/op

BenchmarkRadixSet-4 30000 86513 ns/op

BenchmarkRadixGet-4 30000 85878 ns/op

BenchmarkRadixRedisIncr-4 30000 84889 ns/op

BenchmarkRadixRedisLPush-4 30000 86884 ns/op

BenchmarkRadixRedisLRange10-4 30000 96289 ns/op

BenchmarkRadixRedisLRange100-4 20000 153149 ns/op

对比可以看出，redisgo更好一些

