---
title: Git 系列（一）|基础
date: 2018-03-27 14:46:05
tags:
description: 介绍 github 管理工具 git 的入门常用操作，包括下载、安装等。
---

![](http://pba3iv8pa.bkt.clouddn.com/gitlogo.jpg)

### 一、下载window/linux/mac
### 二、注册Github账户
### 三、基本使用（window）
```
1. 新建文件夹和文件
2. git status // 查看git仓库状态，任何指令前都要带git
3. git init //初始化仓库，对于新建的文件夹和文件，需要执行这一步，才成为一个git仓库
4. git add // 将新文件增加到仓库，相当于“暂存区”
5. git commit -m "first commit" //提交文件到仓库，真正的本地仓库
6. git log //查看所有的commit记录
7. git branch //建立分支
8. git checkout a //切换分支
```
### 四、向Github提交代码
```
1. SSH授权，在github上添加ssh key配置（ id_rsa.pub公钥 ）
2. 生成SSH key,在终端里输入ssh验证是否有安装
3. 输入 ssh-keygen -t rsa ，什么意思呢？就是指定 rsa 算法生成密钥，接着连续三个回车键（不需要输入密码），然后就会生成两个文件 id_rsa 和 id_rsa.pub ，而 id_rsa 是密钥，
id_rsa.pub 就是公钥。这两文件默认分别在如下目录里生成：向GitHub 提交代码Linux/Mac 系统 在 ~/.ssh 下，win系统在 /c/Documents and Settings/username/.ssh下，都是隐藏文件，相信你们有办法查看的。接下来要做的是把 id_rsa.pub 的内容添加到 GitHub 上，这样你本地的 id_rsa 密钥跟 GitHub上的 id_rsa.pub 公钥进行配对，授权成功才可以提交代码。
4. git push origin master //推送代码到远程github仓库
5. git pull origin master //更新远程代码到本地
6. git clone git@github.com:rainzhou/test.git //克隆github项目到本地，修改后push即可
7. git remote add origin git@github.com:rainzhou/test.git //添加一个远程仓库，他的地址是 git@github.com:rainzhou/test.git ，而 origin 是给这个项目的远程仓库起的名字
8. git remote -v //查看关联的仓库有哪些
9. git push origin master 提交
```