---
title: JavaScript 之 rem 的动态控制
date: 2017-02-05 11:07:04
tags:
description: 在写响应式H5页面的时候，我常常会用rem字体，为了兼容多个分辨率的设备，需要写多个 @media 标签，太麻烦并且不够精致，尤其是移动端的页面往往达不到我想要的效果，后来就用Js 替代了css的 @media，我觉得挺好用。
---
摘要：在写响应式H5页面的时候，我常常会用rem字体，为了兼容多个分辨率的设备，需要写多个@media标签，太麻烦并且不够精致，尤其是移动端的页面往往达不到我想要的效果，后来就用js替代了css的@media，下面是代码。（以下方案以750px的设计图）

**方案一、缺点：浏览器里使用没问题，安卓和ios设备hybrid app里嵌入的时候，会受到系统字体设置的影响，原理就是当系统设置超大字体的时候，会改变html的font-size大小，引起页面bug。**
```javascript
(function (doc, win) {
    var docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function () {
            var clientWidth = docEl.clientWidth;
            if (!clientWidth) return;
            if(clientWidth>=750){
                docEl.style.fontSize = '100px';
            }else{
                docEl.style.fontSize = 100 * (clientWidth / 750) + 'px !important';
            }
        };
    if (!doc.addEventListener) return;
    win.addEventListener(resizeEvt, recalc, false);
    doc.addEventListener('DOMContentLoaded', recalc, false);
})(document, window);
```
**方案二、重置了系统设置的字体，解决了此bug，暂无发现有其他问题**
```javascript
function htmlFontSize(){ 
    var h = Math.max(document.documentElement.clientHeight, window.innerHeight || 0); 
    var w = Math.max(document.documentElement.clientWidth, window.innerWidth || 0); 
    var width = w > h ? h : w; 
    width = width > 750 ? 750 : width; 
    var fz = ~~(width*100000/37.5)/20000; 
    document.getElementsByTagName("html")[0].style.cssText = 'font-size: ' + fz +"px"; 
    var realfz = ~~(+window.getComputedStyle(document.getElementsByTagName("html")[0]).fontSize.replace('px','')*10000)/10000;   
    if (fz !== realfz) { 
        document.getElementsByTagName("html")[0].style.cssText = 'font-size:' + fz * (fz / realfz)+"px"; 
    } 
}
```
**衍生问题：安卓和ios怎么屏蔽系统字体大小的设置？**

**安卓、WebView加上这个设置后,可以屏蔽系统设置的字体影响，这样就不用js再去判断了。**
```javascript
webview.getSettings().setTextZoom(100);
```

**ios、ios当系统字体变化时，会给body增加新属性 -webkit-text-size-adjust，所以直接css控制下。**
```javascript
body {
    -webkit-text-size-adjust: 100% !important;
}
```