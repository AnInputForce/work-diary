# 基于Visio Studio Code打造go的IDE

[TOC]

# 前言

​	最近有点儿时间，把想做的事情列了个清单。开个新坑，学习下go。主要是想从go入手，实践微服务。把VSC编辑器清理了下，打造go的高效IDE。本命年，学习go，应景：）

​	本文基于macOS High Sierra 10.13.1。

# 配置本地go环境

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
ChinaDreams:~ kangcunhua$ go env # 可以查看已配置的go环境变量
```

## Hello world

### 新建源码目录

```shell
ChinaDreams:work-space kangcunhua$ cd $GOPATH
ChinaDreams:go-project kangcunhua$ mkdir src # go 源码目录
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
ChinaDreams:go-projcet kangcunhua$ tree
.
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

## 安装依赖包排错和解决

提示安装6个tools失败，

```shell
Installing github.com/ramya-rao-a/go-outline FAILED
Installing github.com/acroca/go-symbols FAILED
Installing golang.org/x/tools/cmd/guru FAILED
Installing golang.org/x/tools/cmd/gorename FAILED
Installing sourcegraph.com/sqs/goreturns FAILED
Installing github.com/golang/lint/golint FAILED
```

### 分析尝试手工编译安装

进入目录，发现除golang.org下的包，其余源文件均已get，于是手工编译安装： go build 执行报错，提示

```shell
ChinaDreams:go-outline kangcunhua$ go build
main.go:14:2: cannot find package "golang.org/x/tools/go/buildutil" in any of:
	/usr/local/Cellar/go/1.10/libexec/src/golang.org/x/tools/go/buildutil (from $GOROOT)
	/Users/kangcunhua/Documents/work-space/go-project/src/golang.org/x/tools/go/buildutil (from $GOPATH)
```

### git clone golang tools 

好吧，先把整个golang tools clone下来再说：

> 其实 golang 在 github 上建立了一个[镜像库](https://github.com/golang)，如 <https://github.com/golang/tools> 即是 <https://golang.org/x/tools> 的镜像库

```shell
ChinaDreams:x kangcunhua$ git clone git@github.com:golang/tools.git
ChinaDreams:tools kangcunhua$ go get -u -v  github.com/golang/tools/go/buildutil
github.com/golang/tools (download)
package github.com/golang/tools/go/buildutil: code in directory /Users/kangcunhua/Documents/work-space/go-project/src/github.com/golang/tools/go/buildutil expects import "golang.org/x/tools/go/buildutil"
```

### 再次手工编译安装

尝试了一个了一个，成功

```shell
ChinaDreams:ramya-rao-a kangcunhua$ cd go-outline/
ChinaDreams:go-outline kangcunhua$ ls
LICENSE		README.md	main.go
ChinaDreams:go-outline kangcunhua$ go build
ChinaDreams:go-outline kangcunhua$ go install
ChinaDreams:go-outline kangcunhua$ 
```

### 切回vsc自动安装

在vsc中的go源码界面按了一下cmd + s保存，提示还缺5个依赖包，选择自动安装，虽然慢点儿，但是也成功了。

```shell
Installing 5 tools at /Users/kangcunhua/Documents/work-space/go-project/bin
  go-symbols
  guru
  gorename
  goreturns
  golint

Installing github.com/acroca/go-symbols SUCCEEDED
Installing golang.org/x/tools/cmd/guru SUCCEEDED
Installing golang.org/x/tools/cmd/gorename SUCCEEDED
Installing sourcegraph.com/sqs/goreturns SUCCEEDED
Installing github.com/golang/lint/golint FAILED

1 tools failed to install.

golint:
Error: Command failed: /usr/local/go/bin/go get -u -v github.com/golang/lint/golint
github.com/golang/lint (download)
Fetching https://golang.org/x/lint?go-get=1
https fetch failed: Get https://golang.org/x/lint?go-get=1: dial tcp 216.239.37.1:443: i/o timeout
package golang.org/x/lint: unrecognized import path "golang.org/x/lint" (https fetch: Get https://golang.org/x/lint?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
github.com/golang/lint (download)
Fetching https://golang.org/x/lint?go-get=1
https fetch failed: Get https://golang.org/x/lint?go-get=1: dial tcp 216.239.37.1:443: i/o timeout
package golang.org/x/lint: unrecognized import path "golang.org/x/lint" (https fetch: Get https://golang.org/x/lint?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
```

### 最后一个手工安装

```shell
ChinaDreams:github.com kangcunhua$ cd golang/lint
ChinaDreams:lint kangcunhua$ go build
ChinaDreams:lint kangcunhua$ go install
```

### 验证依赖包安装结果

切回vsc，源码界面保存，世界清静了

```shell
Finished running tool: /usr/local/go/bin/go vet ./...

Finished running tool: /usr/local/go/bin/go build -i -o /var/folders/0_/rnwscjh51j10hpqykp0rdly00000gp/T/go-code-check opsdev.ren/hellomath/mathapp
```

## 安装项目调试插件

> 推荐brew安装，不用自己配置很多麻烦的东西：

```shell
brew install go-delve/delve/delve
```

### 安装报错

```shell
security: SecKeychainSearchCopyNext: The specified item could not be found in the keychain.
```

### 解决办法

执行脚本导入证书。参考： [mac上 go-delve 安装出现The specified item could not be found in the keychain 解决方法](http://www.cnblogs.com/StephenWu/p/7554393.html)

```shell
ChinaDreams:~ kangcunhua$ cd $HOME/Library/Caches/Homebrew
ChinaDreams:Homebrew kangcunhua$ tar xf delve-*.gz # 解压delve包
ChinaDreams:Homebrew kangcunhua$ cd delve-1.0.0 
ChinaDreams:delve-1.0.0 kangcunhua$ sh scripts/gencert.sh # 执行脚本导入证书
Password:
ChinaDreams:delve-1.0.0 kangcunhua$ brew install go-delve/delve/delve
ChinaDreams:delve-1.0.0 kangcunhua$ dlv version # 验证安装是否成功
Delve Debugger
Version: 1.0.0
Build: v1.0.0
ChinaDreams:delve-1.0.0 kangcunhua$ cd .. 
ChinaDreams:Homebrew kangcunhua$ rm -f -R delve-1.0.0 # 清理工作
```



## 配置调试环境

配置工作区

```json
	// 配置go开发环境 start
    "files.autoSave": "off",
    "go.buildOnSave": true,
    "go.lintOnSave": true,
    "go.vetOnSave": true,
    "go.buildFlags": [],
    "go.lintFlags": [],
    "go.vetFlags": [],
    "go.coverOnSave": false,
    "go.useCodeSnippetsOnFunctionSuggest": false,
    "go.formatOnSave": true,
    "go.formatTool": "goreturns",
    "go.goroot": "/usr/local/go",// 你的Goroot
    "go.gopath": "/Users/kangcunhua/Documents/work-space/go-project",// 你的Gopath
    // 配置go开发环境 end
```

点F5调试，调试控制台输出如下：

```shell
API server listening at: 127.0.0.1:27824
Hello,world. Sqrt(2) = 1.414213562373095
```

配置运行

关于 go run 、build、install的区别，请参考这里：[go run](http://wiki.jikexueyuan.com/project/go-command-tutorial/0.6.html)。

按快捷键cmd + shift + B ，按提示：**配置任务运行程序**—> **Others**

新建task.json

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "go-echo",
            "type": "shell",
            "command": "go",
            "args": [
                "run",
                "${file}"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```

这时，对于可执行的程序文件，按快捷键cmd + shift + B，即可正常执行：

```shell
> Executing task in folder opsdev.ren: go run /Users/kangcunhua/Documents/work-space/go-project/src/opsdev.ren/hellomath/mathapp/main.go <

Hello,world. Sqrt(2) = 1.414213562373095

终端将被任务重用，按任意键关闭。
```

## 用户自定义

关于自动保存方面的个性配置，参考自[这里](http://blog.csdn.net/gnhxsk2015/article/details/74137142)，也可以看[这里](https://studygolang.com/articles/8869)，有更详细的说明。

```json
// 将设置放入此文件中以覆盖默认设置
{
    "workbench.activityBar.visible": true,
    "workbench.sideBar.location": "left",
    // 配置go开发环境 start
    "files.autoSave": "off",
    "go.buildOnSave": true,
    "go.lintOnSave": true,
    "go.vetOnSave": true,
    "go.buildFlags": [],
    "go.lintFlags": [],
    "go.vetFlags": [],
    "go.coverOnSave": false,
    "go.useCodeSnippetsOnFunctionSuggest": false,
    "go.formatOnSave": true,
    "go.formatTool": "goreturns",
    //"go.goroot": "/usr/local/go",// 你的Goroot
    //"go.gopath": "/Users/kangcunhua/Documents/work-space/go-project",// 你的Gopath
    // 配置go开发环境 end    
}
```

# 结束

截至目前，已经就go基础SDK、IDE调试、运行配置完毕。

开启你的基于Visio Studio Code的go开发IDE欢乐之旅吧！

Ps:后续研究下，如何使用远程调试，做一个开发机docker镜像出来，在团队作战中效率会高很多。

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

# Tips2：Homebrew:Mac缺失的软件包管理器

统一维护的软件仓库，丰富可信赖的软件源，替你搞定不少依赖问题。

用rock老师的话说，程序猿不用brew，就错过了mac系统太多的精彩。

## 安装：官方教程

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 检查版本

```shell
ChinaDreams:local kangcunhua$ brew -v
Homebrew 1.5.5
Homebrew/homebrew-core (git revision edac; last commit 2018-02-27)
```

### 卸载

实在搞不定brew问题时，可以使用重装大法。卸载之前，可以看看[官方文档](https://docs.brew.sh/FAQ)；

```shell
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```

## 安装：国内教程

使用官方教程安装都特别慢时，可以参考这篇教程：[Mac下使用国内镜像安装Homebrew](https://www.jianshu.com/p/6523d3eee50d)

- 由于官方弃用了旧的homebrew仓库，择期删除（目前国内镜像已删）
- 将homebrew程序与软件包拆分成了两个仓库（brew.git与homebrew-core.git）

## brew使用

日常使用可以用：

+ brew config 查看配置
+ brew doctor 排查有问题
+ brew help 查看使用教程

### 替换为中科大的源

最新教程参见[官方链接](https://lug.ustc.edu.cn/wiki/mirrors/help/brew.git)

```shell
替换brew.git:
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

替换homebrew-core.git:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```

### 配置二进制源

最新教程参见[官方链接](https://lug.ustc.edu.cn/wiki/mirrors/help/homebrew-bottles)

```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile 
```

### 注意坑

所有网上教程修改源有指向国内homebrew.git镜像的（比如https://git.coding.net/homebrew/homebrew.git），均已过期，会和新版本冲突；不建议尝试。我之前使用的这个镜像，但是brew升级到了新版本，导致冲突，一直报错，找不到origin，尝试修复很久都没搞定。最后只能祭出重装大法。

```shell
ChinaDreams:tt kangcunhua$ brew config
HOMEBREW_VERSION: 1.5.5
ORIGIN: (none) # 找不到origin
HEAD: (none)
Last commit: never
Core tap ORIGIN: (none)
Core tap HEAD: (none)
Core tap last commit: never
...
ChinaDreams:tt kangcunhua$ brew doctor
....
Warning: Missing Homebrew/brew git origin remote.

Without a correctly configured origin, Homebrew won't update
properly. You can solve this by adding the Homebrew remote:
  git -C "/usr/local/Homebrew" remote add origin https://github.com/Homebrew/brew.git
.... # 但是执行此命令会提示 已经存在origin
```

# Tips3：node.js卸载方法收集

引用自[这里](https://www.cnblogs.com/EasonJim/p/6287141.html)。

## brew的安装方式

```Shell
brew uninstall nodejs
```

## 官网下载pkg安装包的

```shell
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```

​	之前测试VUE前端框架，安装过node.js，还降级过版本。这次修复完brew后，发现通过brew安装的新版本不起作用，node -v还是执行旧版本。全卸载了，需要的时候，通过brew安装吧。

# 参考&&引用

+  [Golang 在mac上用VSCode开发、Delve调试](http://www.cnblogs.com/ficow/p/6785905.html)
+ [Go开发：Mac上安装Go环境和VS Code](http://blog.csdn.net/gnhxsk2015/article/details/74137142)
+ [Visual Studio Code折腾记](http://blog.csdn.net/column/details/14326.html)
+ [VS Code开发技巧集锦](http://blog.csdn.net/tiantangyouzui/article/details/52163175)
+ [Visual Studio Code 配置指南](http://blog.csdn.net/GarfieldEr007/article/details/54431261)
+ [用VSCode写python的正确姿势](http://www.cnblogs.com/bloglkl/p/5797805.html)
+ [go get golang.org/x 包失败解决方法](http://blog.csdn.net/alexwoo0501/article/details/73409917)


## 工具相关

+ [使用Homebrew配置Java开发环境](http://www.cnblogs.com/lidyan/p/6973492.html)
+ [使用BFG移除git库中的大文件或污点提交](https://www.awaimai.com/2202.html)
+ [Git 远程仓库 git remote](http://blog.csdn.net/s0228g0228/article/details/45368155)
  + 你可能想要把你的本地的git库，既push到github上，又push到开源中国的Git@OSC上，怎么解决呢
  + git的一个远程库 可以对应多个地址
  + 首先，先增加第一个地址 `git remote add origin <url1>` 
  + 然后增加第二个地址 `git remote set-url --add origin <url2>` 
  + `git push origin master` 就可以一次性push到3各库里面了(使用`git push`也可)