#### 前言
最近在用json格式的配置和网络传输，发现官方的json真的难用啊
本来想造轮子写一个json的解析库，发现github上已经有轮子了
既然已经有轮子了，为啥还要自己早轮子呢
> 此文记录一下两者的使用区别
## encoding/json
本方法是官方的json字符串解析，其与后者相同的地方是都需要转成[]byte来使用
```go
package main

import (
	"fmt"
	"encoding/json"
)

type Data struct {
	num int64 `json:"num"`
}


type Config struct {
	Ip string `json:"ip"`
	Port string `json:"port"`
	Data `json:"data"`
}

func main() {

	sss := `{
		"ip": "127.0.0.1",
		"port": "8080",
		"data":{
			"num":1
		}
	}`
	conf := &Config{}
	_ = json.Unmarshal([]byte(sss),&conf)
	fmt.Println(conf)
}
```
这种方法最蛋疼的一点就是解析的数据需要提前定义好，而且针对key不确定的情况不好处理，所以我更倾向于使用第二种方法
## json-iterator
json-iterator是github上一个的一个轮子，非常的好用，并且在官方表示可以完全替代json！
```go
package main

import (
	"fmt"
	jsoniter "github.com/json-iterator/go"
	//"time"
)
var json = jsoniter.ConfigCompatibleWithStandardLibrary


func main() {

	sss := `{
		"ip": "127.0.0.1",
		"port": "8080",
		"data":{
			"num":1
		}
	}`
	fmt.Println(json.Get([]byte(sss),"data","num").ToInt64())
}
```
在使用Umarshal时候，与官方相同（其实内部应该是调用了官方的解析，但是get方法极其的好用）
其他方法敬请发现
#### 参考文献
[json-iterator官网](https://github.com/json-iterator/go)