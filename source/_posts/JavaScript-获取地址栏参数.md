---
title: JavaScript 获取地址栏参数
date: 2018-07-05 14:47:42
tags:
description: 在 web 开发中，不同的页面间经常会需要参数的传递，比如新闻列表和新闻详情页面，怎么绑定不同的 id 给它们，这时候比较简单的方案就是通过地址栏传输对应的参数 id，具体的实现我封装了两个工具函数。
---

摘要：在 web 开发中，不同的页面间经常会需要参数的传递，比如新闻列表和新闻详情页面，怎么绑定不同的 id 给它们，这时候比较简单的方案就是通过地址栏传输对应的参数 id，具体的实现我封装了两个工具函数。

### 方案一
```JavaScript
function UrlSearch(){

   var name,value; 
   var str=location.href; //取得整个地址栏
        console.log('str地址是：'+str);

   var num=str.indexOf("?"); // 获取查询符？首次出现的位置
        console.log('num =' + num);

   str=str.substr(num+1); //取得所有参数 stringvar.substr(start [, length ]
         console.log('str =' + str);

   var arr=str.split("&"); //以 & 分隔符，分割成各个参数放到数组里
        console.log('arr =' + arr);

   for(var i=0;i < arr.length;i++){ 
        num=arr[i].indexOf("=");  
        console.log('num =' + num);

        if(num>0){
            name=arr[i].substring(0,num);
            console.log('name =' + name);

            value=arr[i].substr(num+1);
            console.log('value =' + value);

            value=decodeURI(value); // 对 value 进行一次 decode 解码，去除空格、符号等的影响
            console.log('value =' + value);

            this[name]=value;
        } 
    } 
};

var requestUrl = new UrlSearch();
```
### 方案二

``` JavaScript
function UrlSearch() {

    var url = window.location.search; //获取 url 中"?"符开始的字符串
    var theRequest = new Object();
    if (url.indexOf("?") != -1) { // 如果 "?" 符存在
        var str = url.substr(1);
        strs = str.split("&");
        for(var i = 0; i < strs.length; i ++) {
            theRequest[strs[i].split("=")[0]] = decodeURI(strs[i].split("=")[1]);
        }
    }
    return theRequest; // 将其作为对象返回
}
UrlSearch();
```

总结： 事实上，很长一段时间我一直用的第一个函数方法，但是它写的并不好，太冗余了，第二个函数明显更简洁了，算是改进版，以后就用这个。