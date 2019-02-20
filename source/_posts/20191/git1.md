---
title: git完全入门（一）
toc: true
date: 2018-12-29 14:52:44
tags: git
categories: 开发者手册
---

Git是一个开源的分布式版本控制系统，与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

<!-- more -->

文档：
<https://git-scm.com/book/zh/v2>

# 一、起步

## 1.1 关于版本控制

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

### 本地版本控制系统 （Distributed Version Control System，简称 DVCS）

用复制整个项目目录的方式来保存不同的版本，或许还会改名加上备份时间以示区别，如VCS。

### 集中化的版本控制系统 （Centralized Version Control Systems，简称 CVCS）

为了让在不同系统上的人协同工作，诸如`CVS`、`Subversion` 以及 `Perforce` 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。

`缺点`： 中央服务器的单点故障、中心数据库所在的磁盘发生损坏，又没有做备份，将丢失所有数据

### 分布式版本控制系统

   客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来，像 Git、Mercurial、Bazaar 以及 Darcs 等。

## 1.2 Git 基础

### 直接记录快照，而非差异比较
其它大部分系统(CVS、Subversion、Perforce等）将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异。
![存储每个文件与初始版本的差异.](/images/20191/git1.png)
`Git` 更像是把数据看作是对小型文件系统的一组快照。如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。
![存储项目随时间改变的快照.](/images/20191/git2.png)

### 近乎所有操作都是本地执行
Git 中的绝大多数操作都只需要访问本地文件和资源，这也意味着你离线或者没有 VPN 时，几乎可以进行任何操作。
举个例子，要浏览项目的历史，Git 不需外连到服务器去获取历史，然后再显示出来——它只需直接从本地数据库中读取。

### 保证完整性
Git 中所有数据在存储前都计算校验和，然后以校验和来引用。
Git 用以计算校验和的机制叫做 SHA-1 散列（hash，哈希）。 这是一个由 40 个十六进制字符（0-9 和 a-f）组成字符串，基于 Git 中文件的内容或目录结构计算出来。 SHA-1 哈希看起来是这样：
```text
24b9da652252987aa493b52f8696cd6d3b00373
```
Git 中使用这种哈希值的情况很多，你将经常看到这种哈希值。 实际上，Git 数据库中保存的信息都是以文件内容的哈希值来索引，而不是文件名。

### 一般只添加数据
你执行的 Git 操作，几乎只往 Git 数据库中增加数据。 很难让 Git 执行任何不可逆操作，或者让它以任何方式清除数据。

### 三种状态
已提交（`committed`）、已修改（`modified`）和已暂存（`staged`）。 已提交表示数据已经安全的保存在本地数据库中。 已修改表示修改了文件，但还没保存到数据库中。 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
![工作目录、暂存区域以及 Git 仓库.](/images/20191/git3.png)

# 二、Git 基础

## 2.2 获取 Git 仓库

### 在现有目录中初始化仓库
```text
git init
```
该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。
如果你是在一个已经存在文件的文件夹（而不是空文件夹）中初始化 Git 仓库来进行版本控制的话，你应该开始跟踪这些文件并提交。
```t
     git add *.c
 git add LICENSE
 git commit -m 'initial project version'
```
### 克隆现有的仓库
使用的命令是"clone"而不是"checkout"。
当你执行 git clone 命令的时候，默认配置下远程 Git 仓库中的每一个文件的每一个版本都将被拉取下来。
```s
git clone https://github.com/libgit2/libgit
```
这会在当前目录下创建一个名为 “libgit2” 的目录，并在这个目录下初始化一个 .git 文件夹，从远程仓库拉取下所有数据放入 .git 文件夹，然后从中读取最新版本的文件的拷贝。
如果你想自定义本地仓库的名字
```s
git clone https://github.com/libgit2/libgit2 mylibgit
```
Git 支持多种数据传输协议。 上面的例子使用的是 https:// 协议，不过你也可以使用 git:// 协议或者使用 SSH 传输协议，比如 user@server:path/to/repo.git 。

## 2.3 记录每次更新到仓库

目录下的每一个文件都不外乎这两种状态：`已跟踪`或`未跟踪`。
已跟踪的文件是指那些被纳入了版本控制的文件，它们的状态可能处于`未修改`，`已修改`或`已放入暂存区`。除已跟踪文件以外的都属于`未跟踪`文件。
![工作目录、暂存区域以及 Git 仓库.](/images/20191/git4.png)

### 检查当前文件状态
查看哪些文件处于什么状态，可以用 git status 命令。

```c
$ git status
On branch master
nothing to commit, working directory clean

当前目录下没有出现任何处于未跟踪状态的新文件,
显示了当前所在分支，并告诉你这个分支同远程服务器上对应的分支没有偏离。
```

现在，让我们在项目下创建一个新的 test 文件。

```c
$ git status
On branch master
Your branch is up to date with 'origin/master'.
Untracked files:
(use "git add <file>..." to include in what will be committed)

    source/images/20191/git4.png
    test.txt

no changes added to commit (use "git add" and/or "git commit -a")

新建的 test 文件出现在 Untracked files 下面。
未跟踪的文件意味着 Git 在之前的快照（提交）中没有这些文件；Git 不会自动将之纳入跟踪范围
```

### 跟踪新文件

使用命令 git add 开始跟踪一个文件。

```c
git add test
```

此时再运行 git status 命令，会看到 README 文件已被跟踪，并处于暂存状态：

```c
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
(use "git reset HEAD /<file>..." to unstage)

   new file:   test.txt

只要在 `Changes to be committed` 这行下面的，就说明是已暂存状态。
```

### 暂存已修改文件

修改了一个名为 CONTRIBUTING.md 的已被跟踪的文件，然后运行 git status 命令,
```c
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
(use "git reset HEAD <file>..." to unstage)

    new file:   CONTRIBUTING.md
    new file:   test.txt

Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
(commit or discard the untracked or modified content in submodules)

    modified:   CONTRIBUTING.md
```

文件出现在 `Changes not staged for commit` 这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。
现在让我们运行 git add 将"CONTRIBUTING.md"放到暂存区，然后再看看 git status 的输出：

```c
    $ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
(use "git reset HEAD <file>..." to unstage)

    new file:   CONTRIBUTING.md
    new file:   test.txt
```

此时，你想要在 CONTRIBUTING.md 里再加条注释， 重新编辑存盘后，准备好提交。 再运行 git status 看看：

```c
    $ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
(use "git reset HEAD <file>..." to unstage)

    new file:   CONTRIBUTING.md
    new file:   test.txt

Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
(commit or discard the untracked or modified content in submodules)

    modified:   CONTRIBUTING.md

怎么回事？ 现在 CONTRIBUTING.md 文件同时出现在暂存区和非暂存区。
实际上 Git 只不过暂存了你运行 git add 命令时的版本， 
如果你现在提交，CONTRIBUTING.md 的版本是你最后一次运行 git add 命令时的那个版本，
```

所以，运行了 git add 之后又作了修订的文件，需要重新运行 git add 把最新版本重新暂存起来：

```c
$ git add CONTRIBUTING.md
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
(use "git reset HEAD <file>..." to unstage)

    new file:   CONTRIBUTING.md
    new file:   test.txt
```

### 状态简览
```c
$ git status -s
AM CONTRIBUTING.md
M docs/search.zip
M source/_posts/20183/pushToGitee.md
M source/_posts/20191/git1.md
A  test.txt
m themes/hexo-theme-Annie
m themes/maupassant
m themes/yilia
?? source/images/20191/git4.png

A:新添加到暂存区中的文件
右边的 M :文件被修改了但是还没放入暂存区
左边的 M :该文件被修改了并放入了暂存区
AM:在工作区被修改并提交到暂存区后又在工作区中被修改了
??:新添加的未跟踪文件
```

### 忽略文件
```c
$ cat .gitignore
*.[oa]
*~

忽略所有以 .o 或 .a 结尾的文件。
忽略所有以波浪符（~）结尾的文件
```

### 查看已暂存和未暂存的修改
用 `git diff` 命令知道具体修改了什么地方。

### 提交更新
每次准备提交前，先用 `git status` 看下，是不是都已暂存起来了， 然后再运行提交命令 `git commit`
这种方式会启动文本编辑器以便输入本次提交的说明。然后出现：
```c
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Your branch is up to date with 'origin/master'.
#
# Changes to be committed:
#      new file:   CONTRIBUTING.md
#      new file:   test.txt
```

你也可以在 commit 命令后添加 -m 选项，将提交信息与命令放在同一行

```txt
$ git commit -m "test 20190102"
[master 9dfcb08] test 20190102
2 files changed, 6 insertions(+)
create mode 100644 CONTRIBUTING.md
create mode 100644 test.txt

当前是在(master)分支提交的
2个文件修订过，6行添加和删改过。
本次提交的完整 SHA-1 校验和是(100644)
```

### 跳过使用暂存区域
使用暂存区域有时略显繁琐，只要在提交的时候，给 `git commit` 加上 `-a` 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤：
```txt
$ git status
On branch master
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git commit -a -m 'added new benchmarks'
[master 83e38c7] added new benchmarks
1 file changed, 5 insertions(+), 0 deletions(-)
```

### 移除文件
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。
如果只是简单地从工作目录中手工删除文件，运行 `git status` 时就会在 `“Changes not staged for commit”` 部分（也就是 未暂存清单）看到：
```txt
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
(use "git push" to publish your local commits)

Changes not staged for commit:
(use "git add/rm <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
(commit or discard the untracked or modified content in submodules)

        deleted:    CONTRIBUTING.md

```

然后再运行 git rm 记录此次移除文件的操作：

```git
admin@llchen10 MINGW64 /d/workspace/isyslh/hexo (master)
$ git rm CONTRIBUTING.md
rm 'CONTRIBUTING.md'

admin@llchen10 MINGW64 /d/workspace/isyslh/hexo (master)
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
(use "git push" to publish your local commits)

Changes to be committed:
(use "git reset HEAD <file>..." to unstage)

        deleted:    CONTRIBUTING.md
```

下一次提交时，该文件就不再纳入版本管理了。 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删还没有添加到快照的数据，这样的数据不能被 Git 恢复。

另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 .gitignore 文件，不小心把一个很大的日志文件或一堆 .a 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 --cached 选项：

```git
git rm --cached README
```

git rm 命令后面可以列出文件或者目录的名字，也可以使用 glob 模式。 比方说：

```txt
git rm log/\*.log
```

删除以 ~ 结尾的所有文件。

```txt
git rm \*~
```

### 移动文件

## 2.3 查看提交历史

`git log` 会按提交时间列出所有的更新，最近的更新排在最上面。
包括SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

| 选项      |    说明 | 
| :--------: | :--------:|
| -(n)  | 仅显示最近的 n 条提交 |  
| --since, --after     |   仅显示指定时间之后的提交。 | 
| --until, --before      |     仅显示指定时间之前的提交。 | 
| --author      |     仅显示指定作者相关的提交。 | 
| --committer      |     仅显示指定提交者相关的提交。 | 
| --grep      |     仅显示含指定关键字的提交 | 
| -S      |     仅显示添加或移除了某个关键字的提交 | 

> e.g. (`q键`退出`git log`)

`git log -p` 用来显示每次提交的内容差异。

`git log -p -2`仅显示最近两次提交

`git log --stat`简略的统计信息

`git log --since=2.weeks`最近两周内的提交

`git log --since=2008-01-15`具体的某一天

`git log --author=chen10221122`显示指定作者的提交

`git log --grep=chen10221122`搜索提交说明中的关键字

`git log -Sfunction_name`找出添加或移除了某一个特定函数的引用的提交

## 2.4 撤消操作

### 撤消操作

带有 --amend 选项的提交命令重新提交：
例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

```git
git commit -m 'initial commit'
git add forgotten_file
git commit --amend
```

最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。

### 取消暂存的文件

`git reset HEAD <file>...` 如：不小心 `git add *`暂存了两个文件,需取消暂存一个：

```git
git status  
```

该命令会提示使用 `git reset HEAD <file>...`:

```git
git reset HEAD CONTRIBUTING.md
```

### 撤消对文件的修改

`输入git status`同样会提示你如何撤消之前所做的修改,根据提示：

```git
git checkout -- CONTRIBUTING.md
```

## 2.5 远程仓库的使用

### 查看远程仓库

`git remote` :列出你指定的每一个远程服务器的简写

`git remote -v`:显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL

### 添加远程仓库

添加一个新的远程 Git 仓库，指定一个简写:

```git
git remote add pb https://github.com/paulboone/ticgit
拉取的时候运行:
git fetch pb
```

### 从远程仓库中抓取与拉取

`$ git fetch [remote-name]`  抓取克隆（或上一次抓取）后新推送的所有工作,不会自动合并或修改你当前的工作

`git pull` 抓取然后合并远程分支到当前分支

### 推送到远程仓库

`git push [remote-name] [branch-name]`

如：将 master 分支推送到 origin 服务器：`git push origin master`

当你和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。 你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。

### 查看远程仓库





















































