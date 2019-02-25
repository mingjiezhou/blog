---
title: require.js 基本用法
date: 2018-03-30 10:21:38
tags:
description: RequireJS是一个非常小巧的JavaScript模块载入框架，是AMD规范最好的实现者之一；最新版本的RequireJS压缩后只有14K，堪称非常轻量；它还同时可以和其他的框架协同工作，使用RequireJS必将使您的前端代码质量得以提升。
---

### 介绍
RequireJS是一个非常小巧的JavaScript模块载入框架，是AMD规范最好的实现者之一。最新版本的RequireJS压缩后只有14K，堪称非常轻量。它还同时可以和其他的框架协同工作，使用RequireJS必将使您的前端代码质量得以提升。

### 特点
1. 模块化加载
2. 防止js加载阻塞页面渲染
3. 使用程序调用的方式加载js，代码更美观

### 基本用法
####正常使用

**index.html**
```
<!DOCTYPE html>
<html>
    <head>
        <script type="text/javascript" src="app.js"></script>
    </head>
    <body>
      <span>body</span>
    </body>
</html>
```
**app.js**
```
function fun1(){
  alert("it works");
}

fun1();
```

#### requirejs用法

先下载[requirejs代码包](http://requirejs.org/)，然后按照下面的方法引用
 **index.html**
```
<!DOCTYPE html>
<html>
    <head>
        <script type="text/javascript" src="require.js"></script>
        <script type="text/javascript">
            require(["app"]);
        </script>
    </head>
    <body>
      <span>body</span>
    </body>
</html>
```
**app.js**
```
define(function(){
    function fun1(){
      alert("it works");
    }

    fun1();
})
```
此时打开页面，发现弹出it works，说明调用成功了，然后发现这个requirejs调用还有一个好处就是，解决了js加载阻塞问题。

### 相关链接：
1. [requirejs菜鸟教程](http://www.runoob.com/w3cnote/requirejs-tutorial-1.html)
2. [requirejs官网](http://requirejs.org/)