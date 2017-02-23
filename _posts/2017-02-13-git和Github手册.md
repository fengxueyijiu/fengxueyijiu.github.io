---
layout:     post
title:      "Git和Github基本操作手册"
subtitle:   "Git and Github basic operating manual"
date:       2017-2-14 12:00:00
author:     "Strom"
header-img: "img/home-bg-art.jpg"
---
Git 及 Github 在工作学习中非常重要，它不仅仅是一个管理存放代码的仓库，更是一个开源的社区。学习Git及Github的操作是首要技能。
[廖雪峰的 Git 教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)

额，文档整理的不一定完全准确，请酌情参考。

## 1.git 安装及默认设置

### 安装 git

在 linux 下直接使用命令：

```
sudo apt-get install git
```

windows 下百度 gitbash ，下载安装即可。

mac 下

```
brew install git
```

### 基本配置

配置个人的用户名称和电子邮件地址：

```
git config --global user.name "newming"
git config --global user.email "977527479@qq.com"
```

可以查看已有的配置信息

```
git config --list
```

## 2.工作流程

- 克隆 Git 仓库作为工作目录（或者自己新建一个工作目录并初始化为Git仓库）。
- 增加资源及修改文件。
- 提交修改。
- 如果他人修改了代码，可以更新资源。

## 3.基本操作命令

### git init

```
mkdir demo && cd demo
git init
```

通过创建 demo 文件夹，`git init` 初始化，新建一个仓库。

### git clone

如果在 github 上已经有了仓库，可以直接通过 `git clone` 将项目 `clone` 到本地。

```
git clone [仓库地址]
```

### git diff

查看做了哪些修改。按 `q` 退出。

### git add

`git add` 可以将文件添加到缓存去，获得 Git 的跟踪。

```
touch a.html
git add a.html
```
也可以通过加参数，将所有的文件添加到缓存区。下面的三种方式效果相同。

```
git add .
git add -A
git add *
```
### git status

`git status` 可以查看当前版本库各个文件的状态。

```
git status
```

### git commit

`git commit` 将缓存区内容添加到仓库中

```
git commit -m '版本留言，尽量写的语义话'
```

### git log

查看提交记录。

### git reset

取消已经缓存的内容。

```
git reset --hard HEAD
```

也可以通过版本号回滚

```
git reset --hard [版本号]
```

### git rm

git rm 命令把一个文件删除，并把它从git的仓库管理系统中移除。

```
git rm readme.md
```

### git checkout

取消对某个文件的修改，通过 `git status` 查看状态，然后执行 `git checkout`

```
git checkout 文件名
```

## 3.推送代码

首先，要在 github 上新建仓库，然后 clone 下来。

### git push

```
*首次推送*
git push -u origin master
*之后可以省略参数*
git push
```

如果没有通过 clone 现有仓库，而是直接在本地 `git init` 的仓库的话，需要先添加远程仓库地址。

```
*为这个仓库添加一个远程地址*
git remote add origin [你的github上的仓库地址]
```

但是这里还有个问题，就是每次 push 都需要输入用户名和密码，很麻烦。这里需要设置下 ssh 。

### 设置 public key

首先需要在本地机器上生成 key。执行

```
ssh-keygen
```

这时，会在 ~/.ssh/ 文件夹之下生成一对 ssh key ，包括一个 public key 和一个 private key 。（如果是windows用户，这个文件一般会在这里：C:\Users\Administrator\.ssh）

复制 public key

```
cat  ~/.ssh/id_rsa.pub
```

将拷贝的 public key 添加在github账户上：

> 右上角点击头像-> 点击settings-> 点击SSH KEYS-> 点击ADD SSH KEYS-> 将获取的public key粘贴于此

## 4.分支操作

创建新分支

```
git branch [yourbranch]
```

切换分支

```
git checkout [yourbranch]
```

或者直接创建一个分支，并且切换过去

```
git checkout -b [yourbranch]
```

删除分支

```
git branch -d [yourbranch]
```

推送分支

```
git push origin [yourbranch]
```

## 5.分支更新及合并

拉取主分支上的更新

```
git pull origin master
```

合并其他分支代码

```
git merge [otherbranch]
```

## 6.结语

额，不完整，不断补充。
