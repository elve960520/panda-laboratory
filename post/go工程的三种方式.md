> Go语言在发明最初仅支持gopath方法，即全局使用同一个库，当然目前已经基本弃用，在多个版本和工程时会出现库版本不对等问题；所以后来官方出了govendor方法，将依赖的库下载到本地目录的vendor文件夹下，但是govendor需要把项目方法gopath下，很不方便；最终目前的官方解决方案为go mod，就我目前使用来说，go mod确实很方便。
## gopath
gopath方法是最初的go语言的推荐方法，需要将工程目录放到$gopath下的src目录，然后使用go build/go run方法进行编译，目录结构类似如下：
```bash
mengfanyu@mengfanyudeMacBook-Pro:~/go$tree -L 2
.
├── bin
│   ├── dlv
│   ├── fillstruct
│   ├── go-outline
│   ├── go-symbols
│   ├── gocode
│   ├── gocode-gomod
│   ├── godef
│   ├── godoctor
│   ├── golint
│   ├── gomodifytags
│   ├── gopkgs
│   ├── goplay
│   ├── gopls
│   ├── goreman
│   ├── gorename
│   ├── goreturns
│   ├── gotests
│   ├── govendor
│   ├── guru
│   ├── impl
│   └── main
├── pkg
│   ├── mod
│   └── sumdb
└── src
    ├── github.com
    ├── golang.org
    ├── gopkg.in
    └── testProject1
```
在目录testproject1中可以新建文件main.go，然后在此目录进行go build，会生成可执行文件，运行。
该方法比较老，所以基本用处不多，建议使用govendor或go mod
## govendor
govendor在go1.5版本中开始被推荐使用，得确方便很多，可以使用其建立工程
#### 安装
```bash
go get -u -v github.com/kardianos/govendor
```
> 注意：这里的govendor是一个第三方工具，目前还没有发现go官方的工具，不过由于适用性，govendor基本就是govendor的工具
**首先确认一点，govendor的项目必须要放在gopath下，否则会编译不通过**
#### 初始化
```bash
cd project
govender init
```
这样一个本地目录就创建好了，可以看到本地有个*vendor*目录，存放的就是各种包
#### 使用
如果使用一个新的库，执行以下步骤：（这里以json-iterator为例）
1. get
```bash
govendor get github.com/json-iterator/go
```
2. add
```bash
govendor add +e
```
3. 使用
此时，在vendor目录下应该就会有github.com这个文件夹，存放依赖的库，结构图如下
```bash
mengfanyu@mengfanyudeMacBook-Pro:~/go/src/testProject2$tree -L 3
.
├── main.go
└── vendor
    ├── github.com
    │   ├── json-iterator
    │   └── modern-go
    └── vendor.json

4 directories, 2 files
```
main.go就可以编写代码使用
```go
import (
	"fmt"
	"github.com/json-iterator/go"
)
```
## go mod
最后一种方法也是我最喜欢的一种方法，就是go mod，并且这个命令也是官方给的
> go mod在官方1.11版本才会出现，并且是官方推荐的，并在1.14版本中官方认为可以用于生产环境中，所以官方还是对go mod很有自信
1. 初始化
go mod初始化方法比较简单，并且对工程文件位置不做要求，所以，放在计算机上任意位置即可，新建一个目录，cd进去，使用以下命令进行初始化
```bash
go mod init
```
此时，就会出现go.mod文件，代表初始化成功
3. 使用
go mod工程在使用时，可以先进行import，然后再下载，在代码中
```go
import (
	"fmt"
	"github.com/json-iterator/go"
)
```
然后在命令行中输入
```bash
go mod tidy
```
这个命令就会自动的将使用的库下载到本地gopath中，并且代码中不用的包也会自动的删掉
## 总结
其实在实际使用过程中还是会遇到很多坑，我最近一周也接触了这几个方式建立工程，记录一下，有问题可以email作者
### 参考文献
[使用vendor管理Golang项目依赖](https://studygolang.com/articles/4266)
[Go module的介绍及使用](https://blog.csdn.net/benben_2015/article/details/82227338)