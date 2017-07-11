# Node.js与Vue.js学习笔记-01

[TOC]

# 开始之前的碎碎念

## 什么是Node.js，为啥要学

Node.js是一个基于Chrome JavaScript运行时建立的平台， 用于方便地搭建响应速度快、易于扩展的网络应用。Node.js 使用事件驱动， 非阻塞I/O 模型而得以轻量和高效，非常适合在分布式设备上运行数据密集型的实时应用。

一个单线程处理高并发大数据的异步变成服务器端 JavaScript 解释器。支持同时数万条链接到一个物理机。

开发效率高，性能好：比如在web开发领域，比起Java、PHP、Python，一个要顶十个；

## 参考

+ [Node.js学习路线图](http://blog.csdn.net/u011537073/article/details/52888292)
+ [Node.js到底是什么](https://www.ibm.com/developerworks/cn/opensource/os-nodejs/)

> [Node.js](http://lib.csdn.net/base/nodejs)的是建立在Chrome V8引擎的Javascript运行环境(runtime)，可方便地构建快速，可扩展的网络应用程序的平台。[node.js](http://lib.csdn.net/base/nodejs)使用事件驱动，非阻塞I/O模型，轻量、高效，可以完美地处理时时数据，运行在不同的设备上。

> 用Nodejs已经1年有余，陆陆续续写了48篇关于Nodejs的博客文章，用过的包有上百个。和所有人一样，我也从Web开发开始，然后到包管 理，再到应用系统的开发，最后开源自己的Nodejs项目。一路走来，Nodejs已经成为我做Web项目的标配。我非常愿意把原[Java](http://lib.csdn.net/base/javase)、PHP的 Web系统向Nodejs迁移，因为1个人可以很容易的完成10个人的活了。

ebay的选择理由：

>+ 动态语言：开发效率非常高，并有能力构建复杂系统，如ql.io。
>+ 性能和I/O负载：Nodejs非常好的解决了IO密集的问题，通过异步IO来实现。
>+ 连接的内存开销：每个Node.js进程可以支持超过12万活跃的连接，每个连接消耗大约2K的内存。
>+ 操作性：实现了Nodejs对于内存堆栈的监控系统。

# 安装和验证[Mac]

## 开发环境推荐

着急的小伙伴直接看这里：[安装配置命令大全](#jump-commandlist)

### 基础工具

| 工具       | 作用                                       | 描述   |
| -------- | ---------------------------------------- | ---- |
| Homebrew | Mac系统下的包管理器，类似于Linux的apt-get,Windows的“控制面板-安装删除应用程序” |      |
| Node.js  | JavaScript运行环境（Runtime），不同系统直接不能直接运行各种编程语言，Runtime类似于各种国际会议上的同声传译 |      |
| npm      | Node.js下的包管理器，类似于Mac下的Homebrew           |      |
| Git      | 分布式版本管理软件                                |      |

### IDE编辑器选择

| 工具            | 作用                             | 描述   |
| ------------- | ------------------------------ | ---- |
| Sublime Text3 | 神级高效IDE，安装配置可以另外写篇文章。可免费用但有提示； |      |
| VSCode        | 拆箱急用高效IDE，简单配置即可。免费；           |      |
| Typora        | MarkDown编辑器，文档不二之选；            |      |

### 扩展阅读：VUE框架

| 工具      | 作用                                       | 描述   |
| ------- | ---------------------------------------- | ---- |
| Vue     | 一个基于MVVM思想设计的前端框架，支持数据绑定和组件化。支持服务器端渲染。   |      |
| webpack | Vue的组件都是通过".vue"来描述的，无法被客户端的浏览器解析，需要被翻译和打包成".js"文件 |      |
| Vue-cl  | 用来生成模版的Vue工程，相当于按照设计好的图纸来盖房子。就是传说中的代码自动生成器：“脚手架”。 |      |

## 安装[Homebrew](https://brew.sh/index_zh-cn.html)

homebrew相当于Ubuntu的apt-get,Redhat的yum：缺失依赖包管理器。

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 验证是否安装成功

```shell
ChinaDreams:~ kangcunhua$ brew --version
Homebrew 1.1.11
Homebrew/homebrew-core (git revision d783; last commit 2017-03-12)
ChinaDreams:~ kangcunhua$ which brew
/usr/local/bin/brew
```

参考：[HomeBrew的安装和简单使用](http://blog.csdn.net/azhou_hui/article/details/49718511)

## 安装Node.js

```shell
brew install node
```

貌似捎带着把brew也update了

### 验证版本

```Shell
ChinaDreams:~ kangcunhua$ node -v
v7.10.0
ChinaDreams:~ kangcunhua$ brew --version
Homebrew 1.2.1
Homebrew/homebrew-core (git revision bf80; last commit 2017-05-21)
```



### Windows下安装

略

## 安装npm

已经在安装node.js时安装成功了

```shell
ChinaDreams:~ kangcunhua$ npm -v
4.2.0
```

### 更改npm为国内镜像

参考：[国内优秀npm镜像推荐及使用](http://riny.net/2014/cnpm/)

配置后验证是否成功

```shell
$ npm config set registry https://registry.npm.taobao.org
$ npm config get registry
https://registry.npm.taobao.org/
```

## 安装git

### 直接安装git

参考：[Mac下git配置](https://my.oschina.net/hexie/blog/780065)，

- GUI和命令行。前者推荐[Source Tree](http://blog.csdn.net/zcube/article/details/47841175)，后者安装[git-osx-installer](https://git-scm.com/download/mac)使用命令行。推荐后者。

### 通过homebrew

待验证；

```shell
brew install git
```

## 安装webpack

```shell
npm install webpack -g
```

## 安装脚手架vue-cli

```shell
 npm install --global vue-cli
```

# 安装配置命令大全

对上文的总结：<a name="jump-commandlist">命令大全</a>

## 基础环境

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  // 安装Mac 包管理软件
brew install git  // 安装git版本管理
brew install node // 安装node.js,同时也安装了npm的js包管理工具
npm -v // 查看npm包版本
npm config set registry https://registry.npm.taobao.org // 更换为国内淘宝镜像 
```

## 安装Vue

```shell
npm install -g vue //全局安装vue 
npm install -g webpack //全局安装webpack 
npm install -g vue-cli //全局安装脚手架vue-cli 
```

初始化及运行

```shell

```



## 填坑:PhantomJS

运行vue时，npm install时提示：缺少PhantomJS，自动安装超时报错

```shell
PhantomJS not found on PATH
Downloading https://github.com/Medium/phantomjs/releases/download/v2.1.1/phantomjs-2.1.1-macosx.zip
Saving to /var/folders/0_/rnwscjh51j10hpqykp0rdly00000gp/T/phantomjs/phantomjs-2.1.1-macosx.zip
Receiving...
  [====------------------------------------] 10%
  
npm ERR! phantomjs-prebuilt@2.1.14 install: `node install.js`
npm ERR! Exit status 1
```

### 解决

参考：[PhantomJS 安装](https://segmentfault.com/a/1190000009020535)

```shell
 brew install phantomjs
```



## 填坑：VSCode的默认调试参数

### Node.js运行警告及其分析

```shell
检测到 Node.js v7.10.0，因此使用旧版协议调试
node --debug-brk=26731 --nolazy hellonode.js 
(node:51083) DeprecationWarning: node --debug is deprecated. Please use node --inspect instead.
Debugger listening on 127.0.0.1:26731
服务运行在  http://127.0.0.1:8888/
```

暂不解决：通过升级node.js到最新8.0还是有提示--debug-brk is deprecated，建议使用—inspec-brk替代；查了一下，需要自定义扩展package.json的script来实现替代VsCode的默认追加参数。暂不研究。

## 填坑：brew 版本管理

老实说，不算一个坑。因为新旧命令的原因，也算是踩了坑。

### 查看版本并升级

```shell
$ node --version
v7.10.0
$ brew list 
icu4c		node		openssl		phantomjs	wget
ChinaDreams:dist kangcunhua$ brew upgrade node
$ node --version
v8.0.0
```

### 切换回旧版本

```shell
$ cd /usr/local/Cellar/node
$ ls
7.10.0	8.0.0_1
$ brew switch node 7.10.0
Cleaning /usr/local/Cellar/node/7.10.0
Cleaning /usr/local/Cellar/node/8.0.0_1
7 links created for /usr/local/Cellar/node/7.10.0
ChinaDreams:node kangcunhua$ node --version
v7.10.0
```

> [参考](http://cnodejs.org/topic/573d1e47b507f69e1dd8a041#573d3e0ffcf698421d20365e)：另homebrew也可以管理node版本的.如果你的/usr/local/Caller/node/文件夹下有多个node版本的话可以使用 `brew switch node 版本号`切换版本，如果你安装的node不在一个文件夹下的话，可以先`brew unlink node4-lts`删除老版本的node软链，再用`brew link node5`就可以切换到新版本.这里的`node4-lts、node5`都是homebrew中node的formula name,可以使用`brew search node`查看可用node formula

### 查看当前已安装版本

brew versions命令已废弃，需要homebrew-version

### 安装指定版本

# IDE:Visual Studio Code安装配置

请前往官网下载：[Visual Studio Code](https://code.visualstudio.com/)

安装：[Visual Studio Code 安装Mac版](http://jingyan.baidu.com/article/414eccf6465cb56b431f0a29.html)

参考：

+ [VSCODE 插件初探,顺便还有博主的写的背景图插件](http://blog.csdn.net/zhuweideng/article/details/52604359)
+ [如何优雅地使用 VSCode 来编辑 vue 文件？](http://clarence-pan.github.io/2017/03/18/edit-vue-file-via-vscode/?utm_source=tuicool&utm_medium=referral)

## 插件推荐

| 插件名                          | 用途       | 描述                                     |
| ---------------------------- | -------- | -------------------------------------- |
| Node.js Modules Intellisense | node.js  | node.js模块自动补全                          |
| Vetur                        | Vue.js插件 | Vue tooling for VSCode.                |
| Go for Visual Studio Code    | go插件     |                                        |
| Python                       | python插件 |                                        |
|                              | Mock.js  | [生成随机数据，拦截 Ajax 请求](http://mockjs.com) |

