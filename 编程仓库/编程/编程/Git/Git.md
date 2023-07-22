# Git 基本介绍
Git 是一个版本管理控制系统（缩写 VCS），它可以在任何时间点，将文档的状态作为更新记录保存起来，也可以在任何时间点，将更新记录恢复回来。

# Git 工作机制
![[imgs/Pasted image 20230402151252.png]]

# Git 和托管中心
代码托管中心是基于网络服务器的远程代码仓库，一般我们简单称为远程库。
局域网：
	GitLab

互联网
	GitHub（外网）
	Gitee 码云（国内网站）

# Git 常用命令
Git 和 linux 的指令是通用的。

| 命令                                 | 作用           |
| ------------------------------------ | -------------- |
| git config --global user.name 用户名 | 设置用户签名   |
| git config --global user. email 邮箱 | 设置用户签名   |
| git init                             | 初始化本地库   |
| git status                           | 查看本地库状态 |
| git add 文件名                       | 添加到暂存区   |
| git commit -m "日志信息" 文件名      | 提交到本地库   |
| git reflog                           | 查看历史记录   |
| git reset --hard 版本号              | 版本穿梭       |

## 设置用户签名
基本语法：
```
git config --global user.name 用户名 
git config --global user.email 邮箱
```
可以进入 Windows 使用目录下查看 `.gitconfig` 文件查看是否设置完成。

签名的作用是区分不同操作者身份。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。Git 首次安装必须设置一下用户签名，否则无法提交代码。

注意：**这里设置用户签名和将来登录 GitHub（或其他代码托管中心）的账号没有任何关系。**

## 初始化本地库
基本语法：
```
git init
```
只有先初始化本地库之后 git 才有权限上传代码。

## 查看本地库状态
基本语法：
```
git status
```

## 添加暂存区
基本语法：
```
git add 文件名
#添加暂存区

git rm --cached 文件名
# 删除暂存区文件
```

## 提交本地库
将暂存区的文件提交到本地库。
基本语法：
```
git commit -m "日志信息" 文件名

git reflog  
#查看提交日志
```

当我们修改某个文件进行提交时，需要再次将修改后的文件进行提交到暂存区。
可以使用 `git status` 指令查看工作区状态。

## 历史版本
基本指令
```
git reflog
查看精简版版本迭代

git log
查看详细版版本迭代
```

### 版本穿梭
返回某个版本的状态。

基本语法：
```
git reset --hard 版本号

```

Git 切换版本，底层其实是移动的 HEAD 指针。

# Git 分支操作
![[imgs/Pasted image 20230402161218.png]]

在版本控制过程中，同时推进多个任务，**为每个任务，我们就可以创建每个任务的单独分支**。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是一个单独的副本。（分支底层其实也是指针的引用）。

## 分支的好处
同时并行推进多个功能开发，提高开发效率。
各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。 

## 分支的操作
![[imgs/Pasted image 20230402162331.png]]

## 修改分支
想要修改某条分支，需要先进入这条分支，然后在改分支修改即可

## 合并分支
基本语法：
```
git merge 分支名
将指定的分支名合并到当前分支。
```

### 产生冲突
冲突产生的表现：后面状态为 MERGING。

冲突产生的原因： **合并分支时，两个分支在同一个文件的同一个位置有两套完全不同的修改。Git 无法替我们决定使用哪一个。必须人为决定新代码内容。**

### 解决冲突
1. 先去编辑有冲突的文件，删除特殊符号，决定要使用的内容。
```
特殊符号：<<<<<<< HEAD 当前分支的代码
======= 间隔符号
合并过来的代码 >>>>>>> hot-fix
```
2. 将修改好后的文件添加为暂存区
3. 将文件提交，此时提交命令不可以带文件名，因为现在正是正在合并。
```
git commit -m "merge hot-fix"
即可
```

# Git 团队协作机制
## 团队内协作
![[imgs/Pasted image 20230402170029.png]]
只有一个团队的开发者才可以进行远程代码的克隆和上传。在自己的本地库进行版本控制。

## 跨团队合作
![[imgs/Pasted image 20230402170427.png]]
当我们 A 需要一个不是本团队的人 C 去进行优化代码时，我们先想 C 的远程库 Fork，这样 C 的远程库就有了 A 远程库的所有代码，然后 C 克隆本地库，进行一个修改和版本控制，修改好后，将代码 push 自己的本地库，然后向 A 的代码库发送 Pull request 请求，A 进行审核，审核通过后，与自己的远程库进行合并等其他操作。

# GitHub 操作
## 创建远程仓库
![[imgs/Pasted image 20230402171440.png]]

![[imgs/Pasted image 20230402171424.png]]

## 远程仓库操作
![[imgs/Pasted image 20230402171534.png]]


### 创建远程仓库别名
基本语法：
```
git remote -v 
#查看当前所有远程地址别名 

git remote add 别名 远程地址
创建制定链接仓库别名
```

![[imgs/Pasted image 20230402171807.png]]

### 推送本地分支到远程仓库
基本语法：
```
git push 别名 分支
```

将 git 和 github 进行链接，上传即可。

### 拉取远程仓库分支到本地
基本语法：
```
git pull 仓库名 分支名
```

### 克隆远程仓库到本地
基本语法：
```
git clone 远程地址
```

小结：clone 会做如下操作:
1、拉取代码。
2、初始化本地仓库。
3、创建别名

克隆和拉取的区别是：克隆是全部的代码，拉取则是某个分支的代码。

## SHH 免密登录
我们可以看到远程仓库中还有一个 SSH 的地址，因此我们也可以使用 SSH 进行访问。 

```
进入家目录

创建.shh目录

运行命令生成.ssh 秘钥目录【注意：这里-C 这个参数是大写的 C】
ssh-keygen -t rsa -C 备注

复制 id_rsa.pub 文件内容，登录 GitHub，点击用户头像→Settings→SSH and GPG keys。

接下来再往远程仓库 push 东西的时候使用 SSH 连接就不需要登录了。

```

# IDEA 集成 Git
## 配置 Git 配置文件
**为什么忽略他们？**
与项目的实际功能无关，不参与服务器上部署运行。把它们忽略掉能够屏蔽 IDE 工具之间的差异。 

**怎么忽略？**
创建忽略规则文件 xxxx. ignore（前缀名随便起，建议是 git. ignore）。
这个文件的存放位置原则上在哪里都可以，为了便于让~/. gitconfig 文件引用，建议也放在用户家目录下 
git. ignore 文件模版内容如下：
```
######################
# 解决java产生文件
######################
*.class

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.ear

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*

######################
# 解决maven产生的文件
######################

target/
**/target/
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup
pom.xml.next
release.properties
dependency-reduced-pom.xml
buildNumber.properties
.mvn/timing.properties

######################
# 解决各类编辑器自动产生的文件
######################

*.iml

## Directory-based project format:
.idea/
# if you remove the above rule, at least ignore the following:

# User-specific stuff:
# .idea/workspace.xml
# .idea/tasks.xml
# .idea/dictionaries

# Sensitive or high-churn files:
# .idea/dataSources.ids
# .idea/dataSources.xml
# .idea/sqlDataSources.xml
# .idea/dynamic.xml
# .idea/uiDesigner.xml

# Gradle:
# .idea/gradle.xml
# .idea/libraries

# Mongo Explorer plugin:
# .idea/mongoSettings.xml

## File-based project format:
*.ipr
*.iws

## Plugin-specific files:

# IntelliJ
/out/
/target/

# mpeltonen/sbt-idea plugin
.idea_modules/

# JIRA plugin
atlassian-ide-plugin.xml

# Crashlytics plugin (for Android Studio and IntelliJ)
com_crashlytics_export_strings.xml
crashlytics.properties
crashlytics-build.properties
```
在. gitconfig 文件中引用忽略配置文件
```
[user]
	name = zml
	email = 320961101@qq.com
[core]
excludesfile = E:/git/git.ignore
注意：这里要使用“正斜线（/）”，不要使用“反斜线（\）”
```


## 定位 GIt 文件 
最好设置为全局设置
![[imgs/Pasted image 20230402181715.png]]
## 初始化本地库
![[imgs/Pasted image 20230402181754.png]]

默认选择的是项目的根目录，一般直接 ok 即可。
![[imgs/Pasted image 20230402181807.png]]


## 添加缓存区
右键项目选择 Add 即可
![[imgs/Pasted image 20230402185351.png]]

提交本地库选择 commint 即可
![[imgs/Pasted image 20230402190338.png]]

## 切换版本
点击 log 日志即可查看版本
![[imgs/Pasted image 20230402190418.png]]

右键选择想要追溯的版本，选择即可
![[imgs/Pasted image 20230402190603.png]]

## 创建分支
右键需要创建的项目，
![[imgs/Pasted image 20230402190803.png]]

![[imgs/Pasted image 20230402191025.png]]


右下角也可以进行传进分支和切换。
![[imgs/Pasted image 20230402191104.png]]

## 合并分支
你想合并那个目录就在那个目录，点击右下角，选择即可。
![[imgs/Pasted image 20230402192029.png]]

### 解决冲突合并
![[imgs/Pasted image 20230402192840.png]]

## Idea 集成 github
添加个人用户即可。
![[imgs/Pasted image 20230402194049.png]]

## 分享工程到 github
1.
![[imgs/Pasted image 20230402194144.png]]

2. 创建即可
![[imgs/Pasted image 20230402194239.png]]

### push 推送本地库到远程库
右键点击项目，可以将当前分支的内容 push 到 GitHub 的远程仓库中。
![[imgs/Pasted image 20230402221246.png]]
注意：push 是将本地库代码推送到远程库，如果本地库代码跟远程库代码版本不一致， push 的操作是会被拒绝的。
也就是说，要想 push 成功，一定要保证本地库的版本要比远程库的版本高！因此一个成熟的程序员在动手改本地代码之前，一定会先检查下远程库跟本地代码的区别！**如果本地的代码版本已经落后，切记要先 pull 拉取一下远程库的代码，将本地代码更新到最新以后，然后再修改，提交，推送！**

### pull 拉取远程库到本地库
右键点击项目，可以将远程仓库的内容 pull 到本地仓库。
![[imgs/Pasted image 20230402221940.png]]

注意：pull 是拉取远端仓库代码到本地，如果远程库代码和本地库代码不一致，会自动合并，如果自动合并失败，还会涉及到手动解决冲突的问题。

### clone 克隆远程库到本地
点击 clone 
![[imgs/Pasted image 20230402222327.png]]

填写仓库即可。
![[imgs/Pasted image 20230402222412.png]]

# 国内代码托管中心-码云
众所周知，GitHub 服务器在国外，使用 GitHub 作为项目托管网站，如果网速不好的话，严重影响使用体验，甚至会出现登录不上的情况。针对这个情况，大家也可以使用国内的项目托管网站-码云。 

码云是开源中国推出的基于 Git 的代码托管服务中心， [码云官网](https://gitee.com/) ，使用方式跟 GitHub 一样，而且它还是一个中文网站，如果你英文不是很好它是最好的选择。 

## IDEA 集成码云
### IDEA 安装码云插件
安装码云插件。
在设置里配置账户即可。
![[imgs/Pasted image 20230402223430.png]]

码云操作和 github 相同。

### 码云复制 github 项目
码云在新建仓库时，支持直接导入 github 项目。
码云也支持项目进行手动更新。 

# 自建代码托管平台-GitLab

## GitLab 简介
GitLab 是由 GitLabInc. 开发，使用 MIT 许可证的基于网络的 Git 仓库管理工具，且具有 wiki 和 issue 跟踪功能。使用 Git 作为代码管理工具，并在此基础上搭建起来的 web 服务。
GitLab 由乌克兰程序员 DmitriyZaporozhets 和 ValerySizov 开发，它使用 Ruby 语言写成。后来，一些部分用 Go 语言重写。截止 2018 年 5 月，该公司约有 290 名团队成员，以及 2000 多名开源贡献者。GitLab 被 IBM，Sony，JülichResearchCenter，NASA，Alibaba， Invincea，O’ReillyMedia，Leibniz-Rechenzentrum (LRZ)，CERN，SpaceX 等组织使用。

## GitLab 官网地址
官网地址： https://about.gitlab.com/
安装说明： https://about.gitlab.com/installation/

## GitLab 安装
使用宝塔可以快速安装。

## Idea 集成 GitLab
下载插件 GitLab Project

设置 GItLab 连接即可拉取项目。