# 基于Visio Studio Code打造go的IDE

[TOC]

# 前言

​	最近有点儿时间，把想做的事情列了个清单。开个新坑，学习下go。主要是想从go入手，实践微服务。把VSC编辑器清理了下，打造go的高效IDE。

​	本文基于macOS High Sierra 10.13.1。

# 配置本地环境

## 软件下载和安装

### go SDK

[官方网站](https://golang.org)貌似被墙了，找了国内的go学习社区[下载](https://studygolang.com/dl)。双击安装。

### 校验

```
Last login: Sat Feb 24 15:07:43 on ttys002
ChinaDreams:~ kangcunhua$ go version
go version go1.10 darwin/amd64
```

## 配置开发环境

```shell
ChinaDreams:~ kangcunhua$ vi .bash_profile 
ChinaDreams:~ kangcunhua$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/go/bin
ChinaDreams:~ kangcunhua$ . .bash_profile # source .bash_profile 重载使配置生效
ChinaDreams:~ kangcunhua$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/go/bin:/Users/kangcunhua/Documents/work-space/go-project/bin
ChinaDreams:~ kangcunhua$ more .bash_profile 
# cofing go-project env 
export GOPATH=$HOME/Documents/work-space/go-project
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOBIN
```

### go env

配置前

```shell
Last login: Sat Feb 24 15:07:54 on ttys002
ChinaDreams:~ kangcunhua$ go ENV
go: unknown subcommand "ENV"
Run 'go help' for usage.
ChinaDreams:~ kangcunhua$ go env
GOARCH="amd64"
GOBIN=""
GOCACHE="/Users/kangcunhua/Library/Caches/go-build"
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/Users/kangcunhua/go"
GORACE=""
GOROOT="/usr/local/go"
GOTMPDIR=""
GOTOOLDIR="/usr/local/go/pkg/tool/darwin_amd64"
GCCGO="gccgo"
CC="clang"
CXX="clang++"
CGO_ENABLED="1"
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/0_/rnwscjh51j10hpqykp0rdly00000gp/T/go-build188026378=/tmp/go-build -gno-record-gcc-switches -fno-common"
ChinaDreams:go-project kangcunhua$ 
```

配置后

```shell
Last login: Sat Feb 24 15:36:58 on ttys001
ChinaDreams:go-project kangcunhua$ go env
GOARCH="amd64"
GOBIN="/Users/kangcunhua/Documents/work-space/go-project/bin"
GOCACHE="/Users/kangcunhua/Library/Caches/go-build"
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/Users/kangcunhua/Documents/work-space/go-project"
GORACE=""
GOROOT="/usr/local/go"
GOTMPDIR=""
GOTOOLDIR="/usr/local/go/pkg/tool/darwin_amd64"
GCCGO="gccgo"
CC="clang"
CXX="clang++"
CGO_ENABLED="1"
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/0_/rnwscjh51j10hpqykp0rdly00000gp/T/go-build975810093=/tmp/go-build -gno-record-gcc-switches -fno-common"
ChinaDreams:go-project kangcunhua$ 
```

## Hello world

### 新建源码目录

```shell
ChinaDreams:work-space kangcunhua$ cd $GOPATH
ChinaDreams:go-project kangcunhua$ mkdir src # go 源码目录
ChinaDreams:go-project kangcunhua$ ls
src
ChinaDreams:go-project kangcunhua$ cd $GOPATH/src
ChinaDreams:src kangcunhua$ mkdir -p opsdev.ren/hellomath
ChinaDreams:src kangcunhua$ cd opsdev.ren/hellomath/
ChinaDreams:hellomath kangcunhua$ vi sqrt.go
ChinaDreams:hellomath kangcunhua$ go install #安装 到$GOPATH/pkg目录
```

### 源码sqrt.go

```go
package mymath
func Sqrt(x float64) float64{
     z := 0.0
     for i := 0; i < 1000; i ++{
        z -= (z * z - x) / (2 * x)
     }
     return z
}
```

### 新建调用应用 main.go

```shell
ChinaDreams:hellomath kangcunhua$ mkdir mathapp
ChinaDreams:hellomath kangcunhua$ cd mathapp
ChinaDreams:mathapp kangcunhua$ vi main.go
ChinaDreams:mathapp kangcunhua$ go build # 该目录下生成一个mathapp的可执行文件 
ChinaDreams:mathapp kangcunhua$ ls
main.go	mathapp
ChinaDreams:mathapp kangcunhua$ ./mathapp # 执行
Hello,world. Sqrt(2) = 1.414213562373095
ChinaDreams:mathapp kangcunhua$ go install # 安装至bin目录(因为package是main)
ChinaDreams:mathapp kangcunhua$ cd $GOPATH/bin
ChinaDreams:bin kangcunhua$ ls
mathapp
ChinaDreams:bin kangcunhua$ mathapp # 直接执行发布的二进制文件
Hello,world. Sqrt(2) = 1.414213562373095
```

### 源码main.go

```go
package main
import(
      "opsdev.ren/hellomath"
      "fmt"
)
func main(){
      fmt.Printf("Hello,world. Sqrt(2) = %v\n",mymath.Sqrt(2))
}
```

### Go 目录介绍

$GOPATH下有bin、pkg、src三大目录。所有活动都在此进行，以刚刚撰写、编译、安装的的helloworld demo为例，目录结构如下，

```shell
ChinaDreams:bin kangcunhua$ tree ..
..
├── bin
│   └── mathapp
├── pkg
│   └── darwin_amd64
│       └── opsdev.ren
│           └── hellomath.a
└── src
    └── opsdev.ren
        └── hellomath
            ├── mathapp
            │   └── main.go
            └── sqrt.go

8 directories, 4 files
ChinaDreams:go-project kangcunhua$ 
```

其中src放置源码

> src目录是开发程序的主要目录，所有的源码是放在这个目录下面。 
> 例如：$GOPATH/src/mymath表示mymath这个应用包或者可执行应用，这个是根据package是main还是其他来决定，main的话是可执行应用，其他的话就是应用包.

pkg放置安装的应用、bin放置可执行的应用；

# 配置Visio Studio Code

## 软件下载和安装

Visio Studio Code[官网下载](https://code.visualstudio.com)，双击安装。

## 安装go插件

在搜索框里输入Go, 找到 Rich Go language support for Visual Studio Code的插件, 点击安装，重启编辑器。

## 自动安装依赖

拖进一个go文件，选择安装全部依赖

```shell
Installing 9 tools at /Users/kangcunhua/Documents/work-space/go-project/bin
  gocode
  gopkgs
  go-outline
  go-symbols
  guru
  gorename
  godef
  goreturns
  golint
```

## 安装项目调试插件

```shell
brew install go-delve/delve/delve
```

参考 [Golang 在mac上用VSCode开发、Delve调试](http://www.cnblogs.com/ficow/p/6785905.html)

# Tips1：关于移除git污点儿的实践

## 基础阅读

+ [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)官网；
+ [BFG作者访谈：使用Roberto Tyley的BFG Repo-Cleaner移除git库中的二进制文件](http://www.infoq.com/cn/articles/git-Cleaner)
+ [如何清洗 Git Repo 代码仓库](http://www.cnblogs.com/developer-ios/p/6211903.html)：这篇文章阐述了为什么要使用BFG ；
+ [Git从库中移除已删除大文件](http://blog.csdn.net/zcf1002797280/article/details/50723783)：这篇文章阐述了传统命令式如何做的，实际上我尝试了下，未竟全功：历史commit的引用没删除掉，不知是不是使用有差错。不过文章阐述的排查大文件的知识点儿还是很有用的；命令例子如下：

```shell
git gc
git count-objects -v
git verify-pack -v .git/objects/pack/pack-8eaeb...9e.idx | sort -k 3 -n | tail -3
git rev-list --objects --all | grep 185ab8d
git log --pretty=oneline --branches -- spark-assembly-1.3.1-hadoop2.4.0.jar
git filter-branch --index-filter 'git rm --cached --ignore-unmatch  spark-assembly-1.3.1-hadoop2.4.0.jar' -- 646784d95f347749517a67c50c117f4bf85d0b42..
rm -Rf .git/refs/original
rm -Rf .git/logs/
git gc
git count-objects -v
```

## 安装java环境

工具基于Java运行。

```shell
ChinaDreams:Downloads kangcunhua$ brew cask install java8
....
==> installer: The install was successful.
🍺  java8 was successfully installed!
ChinaDreams:Downloads kangcunhua$ java -version
java version "1.8.0_162"
Java(TM) SE Runtime Environment (build 1.8.0_162-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.162-b12, mixed mode)
```

## 删除误提交的文件

```shell
ChinaDreams:Downloads kangcunhua$ java -jar bfg-1.13.0.jar --delete-files 201710-kangcunhua体检报告.pdf ../Documents/work-diary
ChinaDreams:Downloads kangcunhua$ cd ../Documents/work-diary
ChinaDreams:work-diary kangcunhua$ git reflog expire --expire=now --all && git gc --prune=now --aggressive
```

## 提交到github

```Shell
ChinaDreams:work-diary kangcunhua$ git push
Everything up-to-date
```

查看了github，搞定。昨天尝试了各种办法，都没删掉的commit黑历史，终于清静了。据info对作者15年1月的访谈中，作者提到“基于下载量来做个估算，据此我猜测，BFG自发布以来，已经为大家节省了大约30人年的工作量。”

从我的尝试来看，速度和效率果然不是吹的，d=====(￣▽￣*)b厉害。

# 参考&&引用

+ [Go开发：Mac上安装Go环境和VS Code](http://blog.csdn.net/gnhxsk2015/article/details/74137142)
+ [Visual Studio Code折腾记](http://blog.csdn.net/column/details/14326.html)
+ [VS Code开发技巧集锦](http://blog.csdn.net/tiantangyouzui/article/details/52163175)
+ [Visual Studio Code 配置指南](http://blog.csdn.net/GarfieldEr007/article/details/54431261)
+ [用VSCode写python的正确姿势](http://www.cnblogs.com/bloglkl/p/5797805.html)


## 工具相关

+ [使用Homebrew配置Java开发环境](http://www.cnblogs.com/lidyan/p/6973492.html)
+ [使用BFG移除git库中的大文件或污点提交](https://www.awaimai.com/2202.html)