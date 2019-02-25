---
title: API 接口加密方案
date: 2018-07-22 15:09:45
tags:
description: 摘要：这是一套经过验证的，项目接口数据传输方案，最初用在安卓和 iOS App上，后来开发小程序版本为了兼容后端代码，就用了同样的方案，事实上也是可行的，只是把 ajax 改成了小程序封装的wx.request，整体结构整理如下，以做备忘。
---
摘要：这是一套经过验证的, 项目接口数据传输方案，最初用在安卓和 iOS App上，后来开发小程序版本为了兼容后端代码，就用了同样的方案，事实上也是可行的，只是把 ajax 改成了小程序封装的wx.request，整体结构整理如下，以做备忘。

**工作方式**

双方通过HTTP方式交互数据，第三方可以简单的 “name=value” 方式发送提交内容或响应请求内容。即通过 HTTP 的 GET/POST 方式交换。

另外双方需要保证数据传输的完整性和安全性，每次发送请求都有响应(响应返回格式为纯文本)，安全验证目前采用用户名和密码的方式。

**字符编码**

服务器接收数据可以是 UTF-8 编码字符,默认接收数据是 UTF-8 编码。

**响应格式**

所谓响应即每次向服务器提交请求后返回值，响应值格式为 JSON 格式，状态码如发送成功后返回{“ret”:”0”,”retMsg “: “提交成功”,”data”:{},”access_token”: “”,”sid”:””}

~~~
ret—返回状态码，所有接口都有返回。
retMsg—状态描述，所有接口都有返回。
sid—会话sessionId，所有接口都有返回。
access_token—与sid对应的令牌，所有接口都有返回，在登陆及注册接口中会返回具体值，其余返回空字符串""。
data—数据，所有接口都有返回，若没有数据则返回{}。
~~~
**接口权限验证传参**

所有请求接口，统一附带下列权限验证参数列表：

参数 | 说明 
---|---
sid	| SessionId
sign | 签名
time_stamp | 时间戳（暂时可不加）

~~~
将 sid 参数和 time_stamp 参数（暂时可不加）带入参数列表
其中参数列表按 key 名根据 ascii 码值进行排序后，生成 json 字符串：
如参数列表：
{"sid":"f0e70fa2ffca4575bf4757a0a3b9f6a6","b":"b","d":"d","a":"a","c":"c"}
排序后生成json字符串为：
{"a":"a","b":"b","c":"c","d":"d","sid":"f0e70fa2ffca4575bf4757a0a3b9f6a6"}
在此字符串基础上加入 access_token 后缀：
假设 access_token 值为 291fb715537cb91463c27b1c19abedfa，则加入 access_token 后的字符串为：
{"a":"a","b":"b","c":"c","d":"d","sid":"f0e70fa2ffca4575bf4757a0a3b9f6a6"}291fb715537cb91463c27b1c19abedfa
根据加入 access_token 后的字符串(假设变量名 str )使用以下方法进行签名 sign 的生成
~~~
**前端 ajax 的封装**

~~~ js
// 定义的封装函数
function postData(data, callback, load) {
	console.log("页面传过来的data", data);
	//data.param={'category.id':9,pageNo :1,pageSize :1}
	encryption(data.param); // 此处调用加密函数，对参数进行加密
	console.log(data.param);
	$.ajax({
		// url: "${pageContext.request.contextPath}/servlet/getCode",
		url: data.url,
		data: data.param, //要用双引号!!
		// contentType: "application/json;charset=utf-8", // 因为上面是JSON数据
		dataType: "json",
		type: data.type,
		headers: {
			// Accept: "application/xml",
			Accept: "application/json",
		},
		beforeSend: function() {
            // 此处可以做一张加载图
			if(load) {
				$("<img class='loader' src='./images/ajax-loader.gif' alt='图片加载失败'/>").appendTo($('body'));
			}
		},
		success: function(data, textStatus) {
			console.log(data);
			if(typeof(data) != "object" && data != "") {
				data = JSON.parse(data);
			}
			if(load) $('.loader').remove();
			callback(data);
		},
		error: function(data, textStatus, errorThrown) {
			console.log(data);
			if(typeof(data) != "object" && data != "") {
				data = JSON.parse(data);
			}
			callback(data);
		},
		complete: function() {
			if(load) {
				$(".loader").remove();
			};
			if($('body').hasClass('bodyTransition')) {
				$('body').removeClass('bodyTransition');
			};
		},
	});
}

// 一般这样调用
var data = {};
// globalAppUrl 是定义的全局域名
data.url = globalAppUrl+"/f/interface/his/doctor/getDoctorList";
data.type = "POST";
data.param = {
    "pageNo":"1",
    "pageSize":"8",
    "isRecommend":"1",
    "hospitalId":sessionStorage.hospitalId,
    "certFlag":"1",
    "serverStatus":"1"
    };
postData(data, function(result) {
    app.professor = result.data.list;
    //alert(JSON.stringify(app.professor))
});
~~~

**加密函数的定义**
~~~ js
function encryption(params){
    // sid 和 accessToken 是调登录接口返回的，加入缓存
    
    // var sid = 'd4dc3aae6c86438eb3fde8ae506e521e';
    // var access_token = '289875947c3bd1d35eb2af26a2543d2e';
    params.sid=sessionStorage.sid;
    console.log(sessionStorage)
    params.time_stamp=new Date().getTime()+'';
    // alert("sid==="+sid + " access_token=="+accessToken);
    var arr = new Array();
    for(var key in params) {
        arr.push(key);
    }
    arr.sort();
    
    var jsonObj = {};
    
    for(var index=0; index<arr.length; index++){
        var key = arr[index];
        jsonObj[key]=params[key];       
    }
    
    // console.log(jsonObj)
    var str = JSON.stringify(jsonObj)+sessionStorage.accessToken;
    // var str = JSON.stringify(jsonObj)+accessToken;
    
    console.info(str)
    var sign=Base.encode(MD5(str)); // 执行 Md5 和 Base64 加密
    console.info(sign)
    params.sign=sign; // 签名 sign 生成完毕，将其与 sid 一同加入参数列表,就可提交请求了
}
~~~

总结：上面是前端部分的基本逻辑，主要有 ajax 的封装和 sign 签名的生成，Java 后端有同样的功能代码进行比对，就不涉及了。