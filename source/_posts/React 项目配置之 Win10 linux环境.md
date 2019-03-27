---
title: React 项目配置之 Win10 linux环境
date: 2019-03-27 20:20:12
tags:
description: 介绍 win10 linux 子系统的安装和 React 项目的启动，实现团队开发环境的统一。
---

团队新启动一个 React 项目， 有些配置命令是 `Linux` 指令，同事用的 Mac 电脑，我 `git clone` 下来之后，发现 `npm install` 和 `npm start` 都有报错，原因是 `package-lock.json` 配置里面有用到软链接的添加和删除（linux指令），而我的是 win10系统，默认不支持，`git bash` 兼容性也不好，莫名报错，为了保持开发环境的一致性，决定在自己本地启用 linux 服务，这次使用 WSL（win10 的 linux 子系统） 来实现。
> Windows Subsystem for Linux（简称 WSL）是一个为在 Windows 10 上能够原生运行 Linux 二进制可执行文件（ELF格式）的兼容层。它是由微软与 Canonical 公司合作开发，目标是使纯正的 Ubuntu 14.04 "Trusty Tahr" 映像能下载和解压到用户的本地计算机，并且映像内的工具和实用工具能在此子系统上原生运行。      
WSL 提供了一个微软开发的 Linux 兼容内核接口（不包含 Linux 代码），来自 `Ubuntu` 的用户模式二进制文件在其上运行。   
该子系统不能运行所有`Linux`软件，例如那些图形用户界面，以及那些需要未实现的 `Linux` 内核服务的软件，不过，这可以用在外部 X 服务器上运行的图形 `X Window` 系统缓解。   
此子系统起源于命运多舛的 Astoria 项目，其目的是允许 Android 应用运行在 Windows 10 Mobile上。   
此功能组件从 `Windows 10 Insider Preview build 14316` 开始可用。   
——维基百科

### 一、安装 linux 子系统

**开启两个 window 权限**
1. 设置 => 更新与安全 => 针对开发人员 => 开发人员模式

<img src="https://user-gold-cdn.xitu.io/2019/3/27/169bed0dcc4f48fa?w=1049&h=646&f=png&s=67669" width = "80%" height = "60%" align="center" />

2. 控制面版 => 程序 => 应用或关闭 Window 功能 => 选中：适用于Linux 的Window 子系统

<img src="https://user-gold-cdn.xitu.io/2019/3/27/169bed1b1ff70a63?w=1178&h=775&f=png&s=124604" width = "80%" height = "60%" />

3. 重启一下电脑~

**安装**

直接打开 window 商店，搜索 linux, 下载、安装、启动；这里我下载的是 `Ubuntu` 版本，也是大家使用最多的一个 WSL 版本。

**更换 Ubuntu 版本的镜像源**

这个主要是解决 `Ubuntu` 系统安装软件时速度慢的问题，毕竟默认镜像源是在国外，你懂的

更换源我找的阿里的，中科大的也可以用，都列出来了
~~~
// 阿里源
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse

// 中科大源
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
~~~
**更换步骤**
~~~
1. 备份原来的数据源配置文件
cp /etc/apt/sources.list /etc/apt/sources.list_backup

2. 编辑数据源配置文件，执行如下指令后继续点击回车键，进入vim 编辑模式，修改（此处可以了解下 vim 常用指令）完成后，Esc 键退出，wq! 指令保存（shift + ':'调出）
vi /etc/apt/sources.list
3. 更新配置
apt-get update
~~~

### 二、配置前端开发环境
要知道，这个 linux 系统是一个独立的系统，我们在 window 上安装的软件如 `Nodejs` 等都需要在 linux 下重新安装才行，下面介绍常用工具的安装方法

**Nodejs的安装**
~~~
1. 使用 curl 命令来下载nodejs 的源码
curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
2. Ubuntu包管理开始安装nodejs
sudo apt-get install -y nodejs（sudo是mac特有的，win下忽略）
~~~
**n 模块的安装与使用**

n 模块是专门用来管理Nodejs 版本的，它的作者同时也是鼎鼎大名的 express 框架的作者，它的特点就是简单，缺点也很明显就是`不支持window 系统`。
~~~
npm install -g n // 全局安装
n latest // 安装最近的正式版
n stable // 安装最近的稳定版
n rm 10.15.2 // 删除某个版本
n use 10.15.2 // 使用某个版本
~~~
**nvm的安装与使用**

其实nvm 和 n模块类似，但是我感觉更好用，可以用它在全局安装多个 Nodejs 版本并随意切换，下面是 linux 下的安装方法（window环境有类似的 gnvm）：
~~~
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash 下载源码

source ~/.bashrc 源码解析安装

nvm --version 有版本信息则说明安装成功

nvm ls 可列出本地所有安装的 Nodejs 版本

nvm ls-remote 可查看所有 Nodejs 版本

nvm install <version>(版本号) 例如：nvm install v10.15.2

nvm alias default v10.15.2 指定默认版本号

nvm -v 查看版本
~~~
**nrm的安装与使用**

nrm 是一个用来管理和快速切换私人 npm 配置的工具，可用来随意的切换 npm 镜像源，比如公司内网镜像，淘宝镜像等，建议全局安装

~~~
npm install nrm -g --save 全局安装

nrm ls 查看默认配置，这里会列出所有的镜像

nrm current 查看当前使用的是哪个源

nrm use <源名> 切换到其它源，如 nrm use cnpm

nrm add 命令可添加公司私有 npm 源，如 http://baozun.com(随便写的)，起个别名叫 baobao，nrm add baobao http://baozun.com
此时再执行 nrm ls，发现 baobao 已添加成功

nrm test npm 测速

nrm del baobao 删除源配置
~~~

![](https://user-gold-cdn.xitu.io/2019/3/27/169bed64bfa36649?w=614&h=212&f=png&s=8946)

好啦，到这里该装的都差不多了，我来试下启动 React 项目:

1. npm install (安装所需依赖)
2. npm start (启动ing)

成功啦！！！

好啦，现在项目成功的在 `linux` 环境下启动，而我的代码编辑工作还是在 `window` 环境，团队开发环境保持了一致，感觉很棒。

参考链接：
1. [Windows10内置Linux子系统初体验](https://www.jianshu.com/p/bc38ed12da1d)
2. [win10下的神器Ubuntu子系统](https://www.jianshu.com/p/9b73e4e0bbb8)
3. [用nrm一键切换npm源](https://www.cnblogs.com/wangmeijian/p/7072053.html)
4. [使用nvm利器，管理node版本](https://www.cnblogs.com/kongxianghai/p/5660101.html)

<div style="text-align:center">
<img src="https://user-gold-cdn.xitu.io/2019/3/27/169beea83d39c070?w=300&h=300&f=png&s=13686" style="width:160px; height:160px;" />
<p style="text-align:center; font-size:13px;">微信扫一扫，公众号：编码美学</p>
</div>
