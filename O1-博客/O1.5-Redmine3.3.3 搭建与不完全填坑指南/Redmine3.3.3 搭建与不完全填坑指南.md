# Redmine3.3.3 搭建与不完全填坑指南

# 环境与搭建

Redmine3.3.3 + SQL Server2012 

为啥不用MySQL，为啥要用SQL Server2012：因为客户习惯；没有选择MySQL还避免踩了一个坑：Redmine3.3.3还暂不支持MySQL5.7(截止现在的最新版本，Redmine3.3.3、Redmine3.3.4和MySQL5.7不兼容)。

## 目的

+ 选用合适的开发辅助工具，项目就成功了一半
+ 精细化管理开发任务
+ 微软的Project跟进太累了，版本难管，分享困难
+ 敏捷开发、快速迭代的需求
+ 方便后续UAT客户参与测试，反馈缺陷和意见，
+ 方便各角色异步协同工作，进度可视化。

## 软件准备

+ RoR环境：点这里下载
+ Redmine3.3.3：点这里下载
+ SQL Server 2012：客户已安装
+ SVN Server：客户已安装

## 安装与配置

# 不联网安装

总是环境需要，不能联网。准确的说是有内网，不能连互联网。工作还是要做的，没有苦难，制造苦难也要上。

还有一种情况是迁移。

## 打包现有应用依赖

## 安装依赖--local

# 填坑清单

## 无法获取speck文件

网络缘故。GFW，或者工作地点的网络有代理。命令行下倒是可以配置代理，只是可惜不知道如何配置代理认证。怎么都连不上网。回家宽带一切都正常了。

## gemfiles中的版本号

（>4.0）指定版本是4.\*，而不能是5.\*。和传统意义上的理解不一样，尤其要注意，在此处栽过不止一次。

## railsInstaller

版本一定要选择3.2.0，只有这个3.2.0符合Redmine3.3的“ ruby2.0，rails4.2”的要求；最新版3.3.0中，rails5.0已经超出了rails（>4.2）要求；

## 系统找不到路径

railsInstaller3.2.0安装后的部分脚本在windows下有路径问题。比如rails.bat ，可以尝试这么解决：

```shell

```



## no such file tiny_tds.0.6.2

又一次，最严重的一次。连接SQL Server时，必须要用到这个gem，但是0.6.2这个版本在windows下有bug，必须至少升级到0.7.0；但是，redmine的gemfile中，直接定义写死了必须是(~>0.6.2)，这就坑了。

果断修改gemfile，去掉版本定义，用最新版。或者改为(~>0.7.0)；

## redmine internal error

访问部分页面，内部错误。这时候要回忆一下，刚刚修改过什么，或者安装过什么插件没。针对性解决之。

一般是没有更新数据库，如下解决：

```shell
rake db:migrate RAILS_ENV=production
```

或者更新插件数据库

```shell
rake redmine:plugins:migrate RAILS_ENV=production
```

## redmine 404 error

查日志：没有找到对应模版。一般是插件安装不对。

## redmine配置SVN后版本库不可以

查日志：ssl连接被拒绝，不信任。

### 配置SVN信任连接

如果你的svn仓库是https://10.29.132.21/prms/trunk/prms/

```shell
svn -l https://10.29.132.21/prms/trunk/prms/
```

p选择永久接受SSL认证即可。

## 局域网连接不上

### 启动参数

在启动命令后，加上以下代码，注意最前方有空格

```shell
 -b 0.0.0.0 -p 3000
```

### 防火墙规则

WinServer2012，防火墙：新建TCP In 规则，开放3000端口即可；

# 安装插件

## Code Review 插件

## Aglie lite 敏捷插件

## 子任务折叠插件

# Windows完整迁移指南

前提：内网环境，另外一个项目需要用到。能否copy一份拿过去简单配置下就能用？

