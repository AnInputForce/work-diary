# 基于Visio Studio Code打造go的IDE

[TOC]

# 前言

​	最近有点儿时间，把想做的事情列了个清单。开个新坑，学习下go。主要是想从go入手，实践微服务。把VSC编辑器清理了下，打造go的高效IDE。

​	本文基于macOS High Sierra 10.13.1。

# 配置本地环境

## 软件下载和安装

### Visio Studio Code

[官网下载](https://code.visualstudio.com)即可。

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

### 校验

git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch *kangcunhua体检报告.pdf'

# 参考

+ [Visual Studio Code折腾记](http://blog.csdn.net/column/details/14326.html)
+ [VS Code开发技巧集锦](http://blog.csdn.net/tiantangyouzui/article/details/52163175)
+ [Visual Studio Code 配置指南](http://blog.csdn.net/GarfieldEr007/article/details/54431261)
+ [用VSCode写python的正确姿势](http://www.cnblogs.com/bloglkl/p/5797805.html)

