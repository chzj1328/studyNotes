# Git从入门到实战

## 一、Git概述

### 1.1、Git简介

Git是一个分布式版本控制工具，通常用来对软件开发过程中的源代码文件进行管理。Git仓库来存储和管理这些文件，Git仓库分为两种：

- 本地仓库：开发人员自己电脑上的Git仓库
- 远程仓库：远程服务器上的Git仓库
- **commit：**提交，将本地仓库文件和版本信息保存到本地仓库
- **push：**推送，将本地仓库文件和版本信息上传到远程残酷
- **pull：**拉取，将远程仓库文件和版本信息下载到本地仓库

![image-20221114161022473](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114161022473.png)



### 1.2、Git下载安装

**下载地址：**[https://git-scm.com/downloads](https://git-scm.com/downloads)

![image-20221114161246220](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114161246220.png)

**【点击next】：**

![image-20221114161529903](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114161529903.png)

**【选择安装位置并点击next】：**

![image-20221114161625560](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114161625560.png)

**【选择安装选项并点击next】：**

- 建议选择Git Bash Here(命令行界面)和Git GUI Here(图形化界面)
- 其他的默认即可

![image-20221114161731326](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114161731326.png)

**【点击next】：**

![image-20221114161946550](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114161946550.png)

**【选择命令行的编辑模式并点击next】：**



![image-20221114162037594](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114162037594.png)

**【选择初始化时代码的结构方式并点击next】：**

- 默认是master，也可以选择main

![image-20221114162312621](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114162312621.png)

**【默认即可点击next】：**

![image-20221114162421585](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114162421585.png)

**【默认即可并点击next】：**

![image-20221114162723988](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114162723988.png)

**【默认并点击next】：**

![image-20221114162806672](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114162806672.png)

**【默认并点击next】：**

![image-20221114162946308](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114162946308.png)

**【选择默认点击next】：**

**配置终端模拟器以与 Git Bash 一起使用**

![image-20221114163421698](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114163421698.png)

**【默认并点击next】：**

![image-20221114163540296](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114163540296.png)

**【默认即可并点击next】：**

![image-20221114163624375](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114163624375.png)

**【默认并点击next】：**

![image-20221114163829370](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114163829370.png)

**【直接点击install】：**

![image-20221114163941218](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114163941218.png)

**【安装完成】：**

![image-20221114164404957](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114164404957.png)

## 二、Git代码托管服务

**常用的Git代码托管服务**

![image-20221114164628234](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114164628234.png)



## 三、Git常用命令

### 3.1、Git全局设置

当安装Git后首先要设置用户名称和email地址

**在Git命令行中执行下面的命令：**

- 设置用户信息

  - git config –global user.name "用户名"

  - git config –global user.email "用户邮箱"

- 查看配置信息
  - git config —list

*注意：*上面设置的user.name和user.email并不是我们注册时的用户名和邮箱，此处可以任意设置

### 3.2、获取Git仓库

**获取Git仓库有两种方式：**

- 在本地初始化一个Git仓库（不常用）
- 从远程仓库克隆（常用）

#### **1、在本地初始化Git仓库**

**执行步骤：**

- 在任意目录下创建空目录作为我们的本地Git仓库
- 进入这个目录，点击右键打开Git Bash窗口
- 执行命令==git init==

![image-20221114171331689](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114171331689.png)

![image-20221114171255646](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114171255646.png)

#### 2、从远程仓库克隆

可以通过Git提供的命令从远程仓库克隆，将仓库克隆到本地

命令形式：**git clone 【远程Git仓库地址】**

![image-20221114171813421](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114171813421.png)

### 3.3、工作区、暂存区、版本库概念

- **版本库：**前面看到的.git隐藏文件夹就是版本库，版本库中存储了很多配置信息、日志信息和文件版本信息等
- **工作区：**包含.git文件的目录就是工作区，也称工作目录，主要用于存放开发的代码
- **暂存区：**.git文件夹中有很多文件，其中有一个index文件就是暂存区，也叫做stage。暂存区是一个临时保存修改文件的地方

![image-20221114202215822](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114202215822.png)

### 3.4、Git工作区中文件的状态

Git工作区中的文件存在两种状态：

- **untracked未跟踪（为被纳入版本控制）**
- **tracked已跟踪（被纳入版本控制）**
  - Unmodified未修改状态
  - Modified已修改状态
  - Stage已暂存状态

### 3.5、本地仓库操作

**本地仓库常用命令：**

- git status 			查看文件状态
- git add                 将文件的修改加入暂存区
- git reset               将暂存区的文件取消暂存或者时切换到指定版本
- git commit          将暂存区的文件修改提交到版本库
- git log                  查看日志

**演示代码：**

- git status

我们在我们的本地仓库新建User.java文件，并用git add *添加带暂存区，使用git status查看状态

```sh
MINGW64 /d/gitRepos/hellogit (main)
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   User.java
```

![image-20221114204352832](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114204352832.png)

- git add

我们在本地仓库新建User.xml，并用git status查看状态，然后再用git add添加到暂存区

```sh
MINGW64 /d/gitRepos/hellogit (main)
$ git add * # 或者(git add User.xml)
MINGW64 /d/gitRepos/hellogit (main)
$ git status
On branch main
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   User.java
        new file:   User.xml
```

![image-20221114204826788](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114204826788.png)

- git commit

我们使用commit命令把我们添加到暂存区的文件提交，再使用git status查看文件的状态，就看不到User.java文件了

```sh
MINGW64 /d/gitRepos/hellogit (main)
$ git commit -m "init hellogit" User.java
# -m 参数 我们提交到仓库的描述信息
```

![image-20221114205107197](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114205107197.png)

- git log

当我们操作几次文件之后就可以使用git log来查看我们操作的日志了

![image-20221114205625115](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114205625115.png)

- git reset

我们可以使用该命令来退回到指定的版本，通过上面的git log查看每个版本的唯一标识

```sh
git reset --hard <唯一标识>
```

![image-20221114210134592](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114210134592.png)

### 3.6、远程仓库操作

**远程仓库的常用命令：**

- git remote                         查看远程仓库
- git remote add                 添加远程仓库
- git clone                            从远程仓库克隆
- git pull                               从远程仓库拉取
- git push                             推送到远程仓库        

**代码演示：**

- git remote 

如果想查看已经配置的远程仓库服务器，可以运行git remote命令

```sh
MINGW64 /d/gitRepos/hellogit (main)
$ git remote
origin
MINGW64 /d/gitRepos/hellogit (main)
$ git remote -v
origin  https://github.com/chzj1328/hellogit.git (fetch)
origin  https://github.com/chzj1328/hellogit.git (push)
```

![image-20221114211046975](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114211046975.png)

- git remote add < shortname > < url > 添加一个新的远程仓库

我们再github 建立一个新的仓库，名称和我们本地仓库名称一致，然后使用git remote add让它和远程仓库发生关联

```sh
MINGW64 /d/gitRepos/repo1 (master)
$ git remote add origin https://github.com/chzj1328/repo1.git
MINGW64 /d/gitRepos/repo1 (master)
$ git remote
origin
MINGW64 /d/gitRepos/repo1 (master)
$ git remote -v
origin  https://github.com/chzj1328/repo1.git (fetch)
origin  https://github.com/chzj1328/repo1.git (push)
```

![image-20221114212340483](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114212340483.png)

- git clone < url >:从远程仓库克隆

Git 克隆的是该Git仓库服务器上的几乎所有数据（包括日志信息、历史记录），而不仅仅是赋值工作所需要的文件

```sh
 MINGW64 /d/gitRepos/ch
$ git clone https://github.com/chzj1328/repo1.git
Cloning into 'repo1'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

![image-20221114213204473](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114213204473.png)

- git push [remote-name] [branch-name]:推送远程仓库

将我们本地的文件推送到远程仓库使用该命令

```sh
MINGW64 /d/gitRepos/hellogit (main)
$ git push origin main
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 303 bytes | 303.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/chzj1328/hellogit.git
   04aa34d..516034d  main -> main
```





![image-20221114215441510](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114215441510.png)



- git pull 从远程仓库拉取并合并到本地仓库

git pull [short-name] [branch-name]

![image-20221114221520539](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221114221520539.png)

### 3.7、分支操作

分支使Git使用过程中非常重要的概念。使用分支意味着你可以把你的工作从开发主线上分离出来，以免影响开发主线。

同一个仓库可以创建多个分支，各分支相互独立，互不干扰。

通过git init命令创建本地仓库时默认会创建一个master分支。

**相关命令：**

- git branch                                          查看分支
- git branch [name]                            创建分支
- git checkout [name]                         切换分支
- git push [shortName] [name]        推送至远程仓库分支
- git merge [name]                             合并分支 

**分支操作：**

- 查看分支
  - git branch                                   列出所有本地分支
  - git branch -r                               列出所有远程分支
  - git branch -a                              列出所有本地分支和远程分支

![image-20221115080104506](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115080104506.png)

- 创建分支
  - git branch [name]

![image-20221115080233461](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115080233461.png)

- 切换分支
  - git checkout [name]

![image-20221115080451810](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115080451810.png)

- 推送本地分支到远程仓库
  - git push [shortName] [name]

![image-20221115080827246](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115080827246.png)

![image-20221115080906039](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115080906039.png)

- 合并分支
  - git merge [name]

![image-20221115081938363](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115081938363.png)

在本地仓库可以看到b1.txt已被合并到我们的主分支上了

![image-20221115082000425](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115082000425.png)

- 合并分支冲突

在b1分支和master分支下同时修改b1.txt文件，然后再进行分支合并

- 在master分支下修改文件内容

![image-20221115082626997](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115082626997.png)

- 在b1分支下修改文件内容

![image-20221115083030383](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115083030383.png)

- 合并分支时发生冲突

![image-20221115083328969](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115083328969.png)

- 修改文件内容（不推荐）：**后面集成IDEA时会有另外的方法**

![image-20221115083853189](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115083853189.png)

### 3.8、标签操作

Git标签：指某个分支特定的时间点的状态。通过标签，可以很方便的切换到标记时的状态。

**相关命令：**

- git tag                                                      列出已有的标签
- git tag [name]                                        创建标签
- git push [shortName] [name]             将标签推送至远程仓库
- git checkout -b [branch] [name]         检出标签

![image-20221115084729816](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115084729816.png)

- 检出标签：检出标签是需要创建一个分支指向某个标签
  - git checkout -b [branch] [name]

![image-20221115085128177](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115085128177.png)

## 四、在IDEA中使用Git

### 4.1、在IDEA中配置Git

在IDEA ==》 File ==》settings ==》 VersionControl进行设置

![image-20221115090135982](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115090135982.png)

### 4.2、获取Git仓库

**在IDEA使用Git获取仓库的两种方式：**

- 本地初始化仓库
- 从远程仓库克隆

**从本地初始化仓库**

![image-20221115090748110](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115090748110.png)

**远程仓库克隆**

![image-20221115091336630](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115091336630.png)

在URL里面输入地址即可和导出单本地的文件夹

![image-20221115091658424](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115091658424.png)

### 4.3、本地仓库操作

**相关命令：**

- 将文件加入暂存区

右键某个文件夹或类 ==》 点击Git ==》点击Add 或者 快捷键 ==》 Ctrl + Alt + A

- 将暂存区的文件提交到版本库

直接点击上方小窗口里面的绿色√：![image-20221115093524665](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115093524665.png)

- 查看日志
- ![image-20221115143553949](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115143553949.png)

### 4.4、远程仓库操作

**相关操作：**

- 查看远程仓库
- 添加远程仓库
- 推送至远程仓库
- 从远程仓库拉取

### 4.5、分支操作

- 查看分支

![image-20221115151338094](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115151338094.png)

![image-20221115151359959](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115151359959.png)

- 创建分支

![image-20221115151724601](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115151724601.png)

- 切换分支：点击分支即可切换

![image-20221115152139162](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115152139162.png)

- 推送分支至远程仓库

![image-20221115152007992](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115152007992.png)

- 合并分支

![image-20221115152223157](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221115152223157.png)







