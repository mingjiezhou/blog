---
title: JavaScript 文本框字数动态监测
date: 2017-11-28 18:43:51
tags:
description: 通过js代码实时监测，文本框内容的变化以及长度，下图是一个实际使用场景。
---

通过js代码实时监测，文本框内容的变化以及长度，下图是一个实际使用场景。

 
![](https://images2018.cnblogs.com/blog/1194550/201711/1194550-20171128184815206-1560586040.png)
　　

HTML部分：
``` HTML
<input id="Text1" type="text" onkeyup="TmaxLength(this)"/>
<span id="Counter" style="color: red;">0</span>  
```
JS部分

``` JavaScript
　　//实时更新输入框文字长度
　　function TmaxLength(x) {
　　　　//x.getAttribute是判断是否为DOM结构
　　　　var nMaxLen = x.getAttribute ? parseInt(x.getAttribute("maxlength")) : ""; 
　　　　if (x.getAttribute && x.value.length > nMaxLen) { 
    　　　　x.value = x.value.substring(0, nMaxLen); 
    　　} 
　　　　document.getElementById("Counter").innerHTML = x.value.length; 
　　}
```

注意：对于可编辑div的话，不能用value属性，改为innerText,且会涉及到光标位置的问题，下篇文章会写到光标定位。

 