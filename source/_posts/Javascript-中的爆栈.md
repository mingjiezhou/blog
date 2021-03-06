---
title: Javascript 中的爆栈
date: 2019-03-12 14:18:22
tags: 
description: 什么是爆栈，和内存溢出有什么关系？
---

>js 中的死循环导致了页面卡死，这是内存溢出吗？

答案：不是，这叫 Maximum call stack size exceeded （爆栈），爆栈就是调用栈深度超出限制了，不同的js 引擎对调用栈的深度设定是不同的，这和内存溢出是两码事情。

**什么是爆栈？**

在 javascript 引擎里面，变量的存储方式有栈内存、堆内存两种方式，栈内存分配的空间是固定的，所以用来存放基本类型数据的；堆内存分配的空间是不固定的，引用类型的值都放在堆内存里，这样处理是为了在程序运行时占用的内存达到最小。

当你写了一个调用自身的循环，不久就看到这么一行报错，Uncaught RangeError: Maximum call stack size exceeded；这说明调用栈的次数（或称深度）是有限制的，那么这个call stack size有多少呢？

这个函数可以用来计算当前 javascript 引擎支持多少次递归调用[参考](http://2ality.com/2014/04/call-stack-size.html)

~~~javasctipt
    function computeMaxCallStackSize（）{
        尝试{
            return 1 + computeMaxCallStackSize（）;
        } catch（e）{
            //调用堆栈溢出
            返回1;
        }
    }
~~~

*三个结果：*

Node.js：11034

Firefox：50994

Chrome：10402

在V8上，您可以进行的递归调用的数量取决于两个量：堆栈的大小和堆栈帧的大小（保持参数和局部变量）。您可以通过向computeMaxCallStackSize（）添加局部变量来验证后者- 它将返回一个较小的数字。

**垃圾回收机制**

javscript 引擎提供了自动的内存清理机制，定时或者根据代码执行中预定的收集时间，周期性的对没用的变量进行回收销毁，以防内存泄露和溢出。（判断无用变量的方法主要有标记清除、引用计数）

基本数据类型的变量在当前执行环境结束时销毁，而引用类型不会随执行环境结束而销毁，只有当所有引用它的变量不存在时这个对象才被垃圾回收机制回收。

**合理利用内存**

通常情况下，因为 javascript 引擎运行在浏览器环境，系统所提供的内存空间是有限的，跟桌面应用没法比，这是为了防止一个网页占用太多的内存而导致整个系统崩溃；这个限制要求我们要合理的利用和优化内存的使用，带来更好的性能。通常可行的做法是，当变量不再有用时及时的将其值设置 null, 垃圾收集器将在下次运行时进行销毁。

