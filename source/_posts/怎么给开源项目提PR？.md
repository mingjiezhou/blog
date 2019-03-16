---
title: 怎么给开源项目提PR？
date: 2019-03-16 23:29:36
tags:
description: 教你给开源项目做贡献，提 PR
---

**1. `Fork` 你想要提交 `PR` 的项目**

**2. 下载到本地**

相关步骤如下:

在你需要的文件夹下面，右键使用 `git bash` 命令，打开 `git` 命令框
执行如下指令可将项目代码下载到当前目录
~~~
代码仓库地址为示例
git clone https://github.com/mingjiezhou/javascript-tutorial.git
~~~
打开项目，执行 `git remote -v` 命令，查看与远程仓库的连接

![image](http://poaukth0o.bkt.clouddn.com/clipboard%20%281%29.png)

如图，`origin` 是与自己 github 远程仓库的连接，`upstream` 是与上游的连接（指 fork 项目的来源），与上游的连接需要我们执行这条指令
~~~
git remote add upstream XXX.git
~~~

**3. 创建自己的代码分支**

所有的代码修改都因该在自己建立的分支下进行
~~~
git checkout -b coding // coding 是我随意命名的分支名

~~~
**4. 修改代码**

**5. 提交代码**
~~~
依次执行下面的命令
git status
git add *
git commit -m '描述文字'
git push origin coding:coding //将新建 coding 分支推送到远程，同样命名为 coding
~~~

**6. 提交pr**

在项目主页找到 `Pull requests` 按钮，点击进入详情页面，然后继续点击 `New pull request` 按钮, 如果你的自建分支有提交过的话，系统将自动获取对应 `commit`，填好 pr 描述（一般和本地提交的 commit 一致就行），和描述说明（对上游作者说的话），点击提交就可以了。到这里，你需要做的就已经全部完成了，上游作者将看到你的 pr 提交，并可能将你的代码并入项目中。

![image](http://poaukth0o.bkt.clouddn.com/clipboard%20%282%29.png)


看到这里，你学会了吗？