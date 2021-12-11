---
title: chrome-note
date: 2021-12-09 17:18:21
tags:
---
https://blog.csdn.net/qustdong/article/details/46046553

浏览器插件开发通信:
（1）content和background之间的通信
a. content往background发消息
b. background往content发消息

（2）content和popup之间的通信
a. background往content发消息
b. content往background发消息

（3）background和popup之间的通信
a. background往content发消息
b. content往background发消息

（一）如何监听数据
答：监听消息是他们的共性，如下：
chrome.extension.onMessage.addListener(
function(request, sender, sendResponse) {
alert("前端/后端/Popup收到");
sendResponse("popup返回值");
}
);


（二）如何发送数据:分为两种方式
一种是给background和popup发送数据，一种是给前端发送数据。
因为前端可能有好几个tab，需要指定给哪个tab发送，所以不能简单的发送。
给background或popup发送数据：
chrome.extension.sendMessage(  {
cmd: "来自前台页面的主动调用"
}, function(response) {
console.log(response);
}
); // 测试前台调后台

给content(前端)发送数据：
chrome.tabs.query({active: true, currentWindow: true}, function(tabs){  
chrome.tabs.sendMessage(tabs[0].id, {message:"calculate"}, function(response) {
if(typeof response !='undefined'){
alert(response);
}else{
alert("response为空=>"+response);
}
});//end  sendMessage   
}); //end query

给content发送数据，不会有问题，那给background或popup发送数据，他们怎么识别是要发给background还是popup呢？
通过测试发送，在content通过chrome.extension.sendMessage发送数据，在background和popup（如果有打开的话）都能收到消息，所以建议在传输数据时，标记下发给谁。

（三）另外说明下，在popup可以直接调用background的方法，如下：

// 先获取background页面
var bg = chrome.extension.getBackgroundPage();
//再在返回的对象上调用background.js 里面的函数
bg.startBrowserAction();


## 监听response headers接收事件 
chrome.webRequest.onHeadersReceived.addListener

```javascript

chrome.webRequest.onHeadersReceived.addListener(
  function (details) {
    var headers = details.responseHeaders;
    for (var i = 0; i < headers.length; ++i) {
      // 移除X-Frame-Options字段
      if (headers[i].name.toLowerCase() === "x-frame-options") {
        headers.splice(i, 1);
        break;
      }
    }
    return { responseHeaders: headers };
  },
  {
    urls: ["<all_urls>"],
   types: ['main_frame', 'sub_frame']
  },
  ["blocking", "responseHeaders"]
);

```

##  chrome.tabs.executeScript

##  chrome.tabs.insertCSS

##  chrome.tabs.get

##  chrome.storage.sync.set({ wx_key: resp.data.appid }, function () { });

## chrome.extension.onMessage.addListener

## chrome.tabs.onActivated.addListener

## chrome.tabs.onUpdated.addListener

## chrome.webRequest.onBeforeRequest.addListener


