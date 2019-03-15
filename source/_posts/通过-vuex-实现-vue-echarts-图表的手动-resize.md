---
title: '### 通过 vuex 实现 vue-echarts 图表的手动 resize'
date: 2019-03-15 19:29:44
tags:
description: 介绍 原生JS 监听 DOM 变化的方法，以及 vue-charts 的手动 resize 方法 
---

#### 背景介绍：

项目有用到 `vue-echarts`, 百度推出的 `vue` 版本的 `Echarts`，图表自带响应式属性 `auto-resize`, 来实现窗口尺寸变化时，图表的尺寸自适应，但是发现它是靠监听 `window` 的 `onresize` 来实现的，而有时候当 `chart` 容器 尺寸变化时，`window` 窗口大小是不变的，比如我这次遇到的，侧边菜单栏的显示隐藏切换，导致内容区域整体部分宽度会变化，但是 `window` 没有变化，所以 `auto-resize` 不能监听这种情况来实现 `resize`。

#### 解决思路：

**一、能否模拟监听普通 Dom 的尺寸变化，然后调用 `echarts` 的 `resize` 事件**

具体做法有以下几种：
1. 初始化项目后，轮询，反复查看 Dom 尺寸是否变化，这种一听就感觉不好，开销太大。
2. 监听元素的滚动事件，在 目标 Dom 里面包裹一个同等大小的 div，是隐藏不可见的，当目标 Dom 大小变化时，触发滚动事件。[参考文章](https://blog.crimx.com/2017/07/15/element-onresize/)
3. 通过 `MutationObserver` 监听 Dom 节点变化，`MutationObserver` 是在 DOM4 规范中定义的，它的前身是 `MutationEvent` 事件，该事件最初在 DOM2 事件规范中介绍，到来了 DOM3 事件规范中正式定义，但是由于该事件存在兼容性以及性能上的问题被弃用；可以用它来监听 Dom style 的变化, **Demo 代码**，[文档](http://javascript.ruanyifeng.com/dom/mutationobserver.html)
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<div id="demo" style="background: blue; height: 200px; width: 100%">
		demo 内容
	</div>
	<script>
		var observer = new MutationObserver(function (mutations, observer) {
		    mutations.forEach(function (mutation) {
		        console.log(mutation);
		    });
		});
		var config = {
		    attributes: true,
		    attributeOldValue: true,
		    attributeFilter: [
		        'style'
		    ]
		};
		var el = document.getElementById('demo');
		
		observer.observe(el, config);
	</script>
</body>
</html>
```
4. Ie9-10 默认支持 div 的 `resize` 事件，可以直接通过 `div.attachEvent('onresize', handler)`; 的方式实现;其它浏览器,通过在 div 中添加一个内置 object 元素实现监听,设置 object 元素的 style 使其填充满 div，这样当 div 的 size 发生变化时，object 的 size 也会发生变化,然后监听 object 元素的 `contentDocument.defaultView(window对象)`的 `resize` 事件。[参考文章](https://www.cnblogs.com/zhtui/p/7059943.html)

**二、vue 实现方案**

斟酌了上面几种方案后，结合具体案例，我觉得还是用 `vuex` 写全局数据更好更方便一些，具体步骤如下
1. 侧边栏展开和隐藏按钮点击 - 触发事件 - 修改 `store` - ,在图表组件里面 `watch store` 的变化，然后执行 `resize` 函数。
2. 关于 `resize()`, 因为 `vue-echarts` 插件已经是对 `echarts` 的封装，如果不修改它的代码的话，可以通过 this.$ref 访问子组件的实例，调用它的 resize()。[文档](https://cn.vuejs.org/v2/guide/components-edge-cases.html#%E8%AE%BF%E9%97%AE%E5%AD%90%E7%BB%84%E4%BB%B6%E5%AE%9E%E4%BE%8B%E6%88%96%E5%AD%90%E5%85%83%E7%B4%A0)
```
    // 注意的坑，1.要设置 width 2. width 变化有延迟，需要加个 timeout，不然看不到效果
    this.$refs.chart.resize({width: this.$refs.chart.offsetWidth})
```
#### 总结

通过这篇文章，你因该了解了两个知识点，DOM的变化监控，以及通过 vuex 作为桥梁实现不同模块布局的变化监听，`autoresize` 的拓展实现手动触发；这就够了。