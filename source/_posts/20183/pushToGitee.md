---
title: 如何搭建本网站-详细步骤
toc: true
date: 2018-08-06 16:55:50
tags: 建站
categories: 开发者手册
---

关于本网站如何搭建的

<!-- more -->

# 搭建步骤

- 1 创建仓库
    <https://blog.csdn.net/tzs_1041218129/article/details/53214497>
    <https://www.cnblogs.com/Dreamice/p/7205231.html>
- 2 使用博客插件
    <https://segmentfault.com/a/1190000008074407>
    <https://blog.csdn.net/wkzd2016/article/details/70170786?locationNum=4&fps=1>
- （3 本地git和远程github连接）
    <https://blog.csdn.net/sihai12345/article/details/72379831>

# 项目备份到Gitee

<https://www.cnblogs.com/mei0619/p/8260696.html>

- 错误1：`Everything up-to-date`
    <https://www.cnblogs.com/kevingrace/p/6259905.html>
- 错误2：`Everything up-to-dateBranch 'master' set up to track remote branch 'master' from 'origin'`
    <https://blog.csdn.net/g1036583997/article/details/50532651>

# git基本

## 提交到码云


将add的文件commit到仓库

```text
git commit -m "注释语句"
```

本地的仓库关联到github上

```text
git remote add origin https://gitee.com/icll/hexo-icll
```

上传代码到github远程仓库

```text
git pull origin master
git push -u origin master
```

`git`其实是是一个不用网络的仓库（本地仓库），你也可以把数据`push`到`github`上（远程仓库）。
`pull`和`push`都是本地版本库和远程仓库之间的数据交互。
在你的本地仓库，其实是由两部分组成：

工作区 (`Working Directory`) //看得见的
版本库 (`Repository`) //看不见的

暂存区(`Stage`)
分支 (`branch`)
版本库包含暂存区和分支

流程：
初次提交：

- 通过`git add`将文件 工作区 ---》暂存区 (本地)
- 通过`git commit` 将文件 暂存区 ---》分支 (本地)
- 通过`git push` 将文件 分支 ---》远程库 (github)

提交改动：

- 通过`git commit`将文件 暂存区 ---》分支 (本地)
- 通过`git push` 将文件 分支 ---》远程库 (github)

pull&push

- 通过`git pull` 将文件 远程库 ---》分支 (本地)
- 通过`git push` 将文件 分支 ---》远程库 (github)

而上面的两个操作是需要有改动，有差异才能执行。
所以会提示暂存区和远程库的内容一致。