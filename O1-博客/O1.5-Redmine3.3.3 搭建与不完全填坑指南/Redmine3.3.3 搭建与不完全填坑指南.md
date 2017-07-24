# Redmine3.3.3 搭建与不完全填坑指南

[TOC]

# 概要

Redmine3.3.3 搭建、不完全填坑指南、不联网安装、Windows内网迁移；

# 环境与搭建

Redmine3.3.3 + SQL Server2012 

为啥不用MySQL，为啥要用SQL Server2012：因为客户习惯；没有选择MySQL还避免踩了一个坑：Redmine3.3.3还暂不支持MySQL5.7(截止现在的最新版本，Redmine3.3.3、Redmine3.3.4和MySQL5.7不兼容（[参考](http://www.redmine.org/issues/25342#note-4))。

## 目的

+ 选用合适的开发辅助工具，项目就成功了一半
+ 精细化管理开发任务
+ 微软的Project跟进太累了，版本难管，分享困难
+ 敏捷开发、快速迭代的需求
+ 方便后续UAT客户参与测试，反馈缺陷和意见，
+ 方便各角色异步协同工作，进度可视化。

## 软件准备

+ RoR环境：[点这里下载](https://s3.amazonaws.com/railsinstaller/Windows/railsinstaller-3.2.0.exe)
+ Redmine3.3.3：[点这里下载](http://www.redmine.org/projects/redmine/wiki/Download)
+ SQL Server 2012：客户已安装
+ SVN Server：客户已安装

## 安装

参考：[官方教程](http://www.redmine.org/projects/redmine/wiki/RedmineInstall#Step-5-Session-store-secret-generation)

### 环境和APP

+ 运行railsInstaller3.2.0.exe，安装rails环境；
+ 修改为ruby-china的gem源：参考[这里](https://gems.ruby-china.org)；
+ 解压redmine3.3.0.zip到目标服务器，比如，E:\PM\redmine3.3.0;
  + 找到config/db.yml，配置数据库相关信息；新建数据库用户和数据库实例；

### 联网安装

先安装管理工具bundle：

```shell
gem install bundler
```

联网安装依赖

```shell
cd E:\PM\redmine3.3.0
bundle install --without development test rmagick
```

Session store secret generation

```shell
bundle exec rake generate_secret_token
```

### 数据库初始化

初始化结构

```shell
RAILS_ENV=production bundle exec rails db:migrate
```

装载默认数据

```shell
RAILS_ENV=production REDMINE_LANG=zh bundle exec rake redmine:load_default_data
```

### 启动Redmine

```shell
rails server webrick -e production -b 0.0.0.0 -p 3000
```

admin/admin，根据提示改密码

Ctrl + c停止redmine

### 安装插件

安装需要的即可，依据回忆整理，可能有所出入。

- a_common_libs // 某插件依赖
- advanced_roadmap_v2 // 非常重要的一个插件，有蓝图才有未来
- bestest_timer 工时插件 // 得有配套流程制度支撑；
- custom_menu // 自定义菜单插件
- redmine_code_review插件 // 最好搭配有制度支撑，有人检查
- redmine_agile // 敏捷插件 , 客户喜欢敏捷。还有一款彻底开源的，Redmine Dashboard，没测试。看着像更好的。
- Redmine_subtask_list_accordion // 子任务折叠插件
- Redmine Subtasks Inherited Fields // 子任务继承插件，时间所限，没测试

## 配置

- 新建项目：\*\*\*project
- 新增团队成员
- 配置SVN版本库
- 配置RoadMap、版本、里程碑
- 配置问题类别
- 新建问题或导入问题
- 配置自定义菜单
- 配置常见查询
- 讨论区发帖：Redmine使用要求

### 培训推广

好的工具要配套建章立制，搞好培训和推广。人，尤其是伪新人和老油条，多是没有下限的。伪新人是不知道什么是优秀，老油条是在试探项目混日子的底线。他们不会在意你强调了什么，会care你要检查什么。所以，至少流程制定出来，一定要有人负责，有人检查，有专项的汇报。再忙，听个汇报，做个跟进的时间还是要有的。

在实际使用过程中，对于逾期的计划，不能就放任红着不处理。要有说法啊，是我们的任务计划预估时间不足？还是工程能力的问题或是态度的问题？亦或是你没跟进检查的问题？该调整开发测试计划或调整人力资源倾斜或调整人员进出场都要推进，或落实责任到人，每天开个站立会，就可以了解实际情况，进而做出调整。

踩过坑的人都懂。

# 不联网安装

总是环境需要，不能联网。准确的说是有内网，不能连互联网。工作还是要做的，没有苦难，制造苦难也要上。

还有一种情况是迁移。

参考：[在 Windows 上安裝 Redmine 2.3.0 的筆記](http://crboy.logdown.com/posts/141615-install-redmine-on-windows)

## 安装配置环境

railsInstaller3.2.0 + SqlServer2012 + SVN

## 打包现有应用依赖

进入redmine目录，执行

```shell
bundle package
```

在$REDMINE\vender\cache\目录下所有的gem包都在里面了。

这个文件也需要：$REDMINE\Gemfile.lock，gem文件之间的依赖定义；

## 安装依赖--local

将上述gem包copy到本地比如d:\pm\gem.local目录

```shell
cd d:\pm\gem.local
gem install -l * 
```

1. `Gemfile.lock` 放回 `$REDMINE`
2. 执行以下命令

```
cd $REDMINE  
bundle install --without development test rmagick --local  
```

修改数据库配置，然后就差不多可以运行redmine了。

# 填坑清单

## 无法获取speck文件

```shell
D:\PM>gem sources --add https://gems.ruby-china.org/
Error fetching https://gems.ruby-china.org/:
        no such name (https://gems.ruby-china.org/specs.4.8.gz)
```

网络缘故。GFW，或者工作地点的网络有代理。命令行下倒是可以配置代理，只是可惜不知道如何配置代理认证。怎么都连不上网。回家宽带一切都正常了。

## gemfiles中的版本号

（>4.0）指定版本是4.\*，而不能是5.\*。（>4.2.0）指定版本是4.2.*，而不能是4.3.\*。和传统意义上的理解不一样，尤其要注意，在此处栽过不止一次。比如MSSSQL的驱动包 tiny_tds安装。

### gem常用命令

```shell
gem install [gemname]	// 联网安装指定gem包
gem install [gemname] -v 0.7.0  // 联网安装指定版本的gem包
gem install [gemname] -l // 从本地安装指定gem包
gem uninstall [gemname] // 卸载gem包
gem list --local  // 查看本地安装了哪些gem包
```



## railsInstaller

版本一定要选择3.2.0，只有这个3.2.0符合Redmine3.3的“ ruby2.0，rails4.2”的要求；最新版3.3.0中，rails5.0已经超出了rails（>4.2）要求；

## 系统找不到路径

railsInstaller3.2.0安装后的部分脚本有路径问题。3.1.1一切还正常。

```shell
rails -v
系统找不到指定的路径。
```

可以尝试这么解决：windows下改bat，linux改sh文件：比如rails.bat 。其他命令如bundle等有此问题的，一样的方法修改。[参考](https://stackoverflow.com/questions/35545361/rails-the-system-cannot-find-the-path-specified)

```shell
​```shell
@ECHO OFF
IF NOT "%~f0" == "~f0" GOTO :WinNT
ECHO.This version of Ruby has not been built with support for Windows 95/98/Me.
GOTO :EOF
:WinNT
@"%~dp0ruby.exe" "%~dpn0" %*
```



## no such file tiny_tds.0.6.2

```shell
D:\PM\redmine-3.3.3>bundle exec rake generate_secret_token
rake aborted!
LoadError: cannot load such file -- tiny_tds/tiny_tds
```

又一次，最严重的一次关于版本的碰撞。直接去研究了gem包的管理；

连接SQL Server时，必须要用到这个gem，但是0.6.2这个版本在windows下有bug（[参考](http://www.redmine.org/boards/2/topics/49379?r=49627#message-49627)），必须至少升级到0.7.0；但是，redmine的gemfile中，直接定义写死了必须是(~>0.6.2)，这就坑了。

果断修改gemfile，去掉版本定义，用最新版。或者改为(~>0.7.0)；

这里附上我在官方讨论区给同样碰上此问题的歪果仁的回复。[链接](http://www.redmine.org/boards/2/topics/49379?r=53132#message-53132)

```shell
i meet this error too. redmine-3.3.3 + SQL Server 2012 + railsinstaller-3.2.0.exe.

just to fix it like this step:
1. open redmine-3.3.3\Gemfile,and find A replace to B,
A:
gem "tiny_tds", "~> 0.6.2", :platforms => [:mri, :mingw, :x64_mingw]
B:
gem "tiny_tds", "~> 0.7.0", :platforms => [:mri, :mingw, :x64_mingw]
2. then 
gem install tiny_tds -v 0.7.0 
3. all is OK;

reason：
1. Peter Shannon wrote:

tiny_tds 0.6.0 is bugged with Windows. Install 0.7.0 to fix.

2. gemfile define the tiny_tds allowed between (>=0.6.2,<0.7.0)

and from internet,there is another writing like:
gem "tiny_tds" 
do not define the tiny_tds version，you also can try it；

just fine！
good luck！
```



## 无法创建issue等

安装时没有执行这个命令装载默认配置数据

```sql
RAILS_ENV=production REDMINE_LANG=zh bundle exec rake redmine:load_default_data
```

补救：

点击“管理”链接，会提示你“装载默认数据和流程”，同意即可；

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

```shell
ActionView::Template::Error (undefined method `accessor' for #<ActiveRecord::Type::Value:0xb375628>)
```

尝试运行以下命令解决：

```shell
bundle exec rake db:migrate RAILS_ENV=production
```



## redmine配置SVN后版本库不可以

查日志：ssl连接被拒绝，不信任。

### 配置SVN信任连接

如果你的svn仓库是https://10.29.132.21/prms/trunk/prms/，在redmine安装机子上运行此命令：

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

+ 插件安装失败或不好用

插件安装失败，或者使用起来，部分页面内部错误，可能有以下原因：

+ 代码有缺陷，
+ 比如不支持此版本redmine，
+ 只支持MySQL，不兼容MsSql；比如 chart2；
+ 可能没有运行插件migrate命令
+ 最最没含量的，也是最容易栽跟头的就是：插件目录名字不对。好好看github说明。



# Windows完整迁移指南

Update：2017.07.19验证完毕，略有修订。~~以下只是推理。还有待于验证。可行性比较大。~~

服务器：WinServer 2012;

## 前提

内网环境，另外一个项目需要用到。能否copy一份拿过去简单配置下就能用？

## 准备

### 依赖包gems

参考：[不联网安装，打包现有应用的依赖](## 打包现有应用的依赖)，导出依赖包，压缩为redmine-gems.zip

### rails环境

+ railsInstaller3.2.0.exe
  + 单独备份已有RailsInstaller\Ruby2.2.0\bin目录为RailsInstaller-patch.zip，做为bug修复包；
  + 原因：该版本默认安装好之后，此目录的部分bat文件，有bug，运行命令时提示“系统找不到对应路径”。
+ 或 RailsInstaller安装目录压缩一份为RailsInstaller.zip

### Redmine3.3.3，copy一份，

+ 修改config/database.yml，关于数据库的配置信息：主机、数据库名、用户名、密码；
+ 备份redmine目录下的plugins目录为plugins.zip，清空plugins目录；
+ 打包:整体打包为redmine3.3.3.zip。

### 数据库用户准备

MsSql，新建数据库比如redmine-test，新建新用户比如redmine-tt，确保对目标数据库确保有权限，用户角色勾选db-owner；

测试代码，命令行输入，正常登录即可；

```sql
osql -S localhost -U redmine-tt -P redmine-tt
```

## 安装

### 安装Rails环境

+ 方法一：使用安装包
  + 运行railsInstaller3.2.0.exe，安装rails环境；
  + 如果备份的有RailsInstaller-patch.zip，解压缩覆盖即可；
+ 方法二：使用压缩包
  + 解压缩，比如E:\PM\RailsInstaller目录；
  + 配置用户环境变量Path，指向ruby的路径。比如“E:\PM\RailsInstaller\ruby\bin”

验证环境：

```shell
ruby -v
gem -v
rails -v
git --verision
```

方法一会遇到提示“系统找不到对应路径”，请参考 [系统找不到路径](#系统找不到路径) 解决；

### 部署Redmine

+ 解压redmine3.3.0.zip到目标服务器，比如，E:\PM\redmine3.3.0;
+ 检查plugins目录是否被清空；
+ 也可以再次检查数据库配置文件，命令行检查用户是否有权限；

### 安装gems包

解压redmine-gems.zip到目标服务器，比如，E:\PM\redmine-gems;

本地安装gems包

```shell
cd E:\PM\redmine-gems
gem intall *.gem --local
```

近60个左右，安装时间约30~50分钟；不要问我问什么这么慢，我想静静.....

### 环境依赖安装检查

因为本地已安装完毕gems包，所以这里不会碰上gem包欠缺。如提示缺包，请下载copy过来单独安装；

```shell
bundle install --without development test rmagick
```

### 数据库migrate

```shell
rake db:migrate RAILS_ENV=production
bundle exec rake generate_secret_token  RAILS_ENV=production
```

### 尝试启动redmine

```
rails server webrick -e production -b 0.0.0.0 -p 3000
```

### 访问初始配置redmine

```shell
http://localhost:3000   ##或者 http://yourserverip:3000
```

+ 正常启动后，依据提示，

  + 配置管理员账号。admin/admin，修改默认密码。

  + 管理—>装载默认配置及流程（如不初始化，可能会有创建不了issue等莫名其妙的问题）

  + 一般来说，管理界面出现这个提示，是因为安装时未执行装载默认数据的操作：

  + ```sql
    RAILS_ENV=production REDMINE_LANG=zh bundle exec rake redmine:load_default_data
    ```

### 安装插件

停止redmine，恢复plugins目录内容，

这两条命令都来一遍吧：


```shell
rake redmine:plugins:migrate RAILS_ENV=production
```

```shell
rake db:migrate RAILS_ENV=production
```

再次启动

```shell
rails server webrick -e production -b 0.0.0.0 -p 3000
```

接下来，就是参考配置喽。

# 后续

其实工具并不能自动解决管理紊乱问题，只能提供一定程度地支持。项目管理最大的挑战仍旧来源于人，尤其是干系人。其次在于流程的梳理，职责的划分。风险识别，解决，是每一天的功课。

沟通到位，分工合理，适时跟进，切实调整，配合神器一样的工具Redmine，事半功倍。

准备学习更新下，Docker环境下的安装配置。

